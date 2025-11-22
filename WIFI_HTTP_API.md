**ReaderPaper — HTTP API（Web 前端 开发者参考）**

- 说明: 本文档面向前端工程师，汇总设备中用于 Web 前端交互的 HTTP 接口（由 `WiFiHotspotManager` 和 `ApiRouter` 提供）。包括请求方法、必需/可选参数、响应格式、示例（curl / fetch / XHR）、以及前端实现注意事项（CORS、预检、上传进度、分页）。
- 设备默认热点地址: `http://192.168.4.1`（在 AP 模式下，`WiFiHotspotManager::getIPAddress()` 返回热点地址）

**通用注意**
- CORS: 服务端会在 JSON API 上添加以下响应头：
  - `Access-Control-Allow-Origin: *`
  - `Access-Control-Allow-Methods: GET, POST, OPTIONS, DELETE`
  - `Access-Control-Allow-Headers: Content-Type, X-Requested-With`
- OPTIONS 预检: 当浏览器发起跨域复杂请求（例如带自定义头或 Content-Type 为 JSON）时，浏览器会先发送 `OPTIONS`，服务器已实现对应的 `HTTP_OPTIONS` 返回（通常返回 204）。前端无需特殊处理，浏览器自动发起预检。
- 大文件上传: 服务器以流式方式写入到 SD 卡（使用临时 `.tmp` 文件），会检查设备剩余内存和 SD 卡空间；上传可能被拒绝并返回 4xx/5xx 错误（见下文）。

---

**1) 获取文件列表 — `/list`**
- 方法: GET
- 路径示例:
  - 列出所有: `GET /list`
  - 列出书籍: `GET /list/book`
  - 列出字体: `GET /list/font`
  - 列出图片: `GET /list/image`
- Query 参数 (可选):
  - `page` (整数) — 页码（从1开始），与 `perPage` 一起作用
  - `perPage` (整数) — 每页条数
- 返回:
  - 无分页: 返回 JSON 数组: [{"name":"...","type":"file|dir","size":12345,"isCurrent":0|1,"isIdxed":0|1}, ...]
  - 分页: 返回对象 {"total":N,"page":P,"perPage":M,"files":[...]}（`total` 为总条目数）
- HTTP 示例 (curl):
  ```bash
  curl "http://192.168.4.1/list/book"
  curl "http://192.168.4.1/list/book?page=1&perPage=20"
  ```
- 前端 Fetch 示例:
  ```js
  const res = await fetch('http://192.168.4.1/list/book');
  const data = await res.json();
  // data 可以是数组或分页对象，检查是否有 data.files
  ```
- 注意: 对于 `/list/book`，服务端返回时会避免返回非 `.txt` 文件（兼容性策略）且会标注是否已索引或是否为当前打开的书。

---

**2) 上传文件 — `/upload`**
- 方法: GET (返回上传页面 HTML), POST (multipart/form-data 上传)
- 说明: 上传使用 multipart/form-data 表单提交。前端可使用 `FormData` 或 XMLHttpRequest 发送文件。
- 表单字段: 推荐字段 `tab` 指定目标目录，支持值: `book`, `font`, `image`。服务器会根据 `tab` 决定存储目录（`/book/`, `/font/`, `/image/`）。
- 限制与行为:
  - 流式写入到 SD（临时 `.tmp` 文件），完成后重命名为目标文件。
  - 服务器会检查可用内存（例如 32KB、24KB 阈值）并可能返回错误。
  - 若上传成功，会返回 JSON {"ok":true,"message":"File uploaded successfully"}
  - 若失败，会返回 4xx 或 5xx 状态与 JSON 错误消息。
- curl 示例:
  ```bash
  curl -F "tab=book" -F "file=@/path/to/book.txt" http://192.168.4.1/upload
  ```
- 前端 Fetch 示例 (FormData):
  ```js
  // 假设 fileInput 是 <input type="file">
  const file = fileInput.files[0];
  const fd = new FormData();
  fd.append('tab', 'book');
  fd.append('file', file, file.name);

  const res = await fetch('http://192.168.4.1/upload', {
    method: 'POST',
    body: fd,
    mode: 'cors'
  });
  const json = await res.json();
  console.log(json);
  ```
- Upload 进度（XHR）示例:
  ```js
  const xhr = new XMLHttpRequest();
  xhr.open('POST', 'http://192.168.4.1/upload');
  xhr.upload.onprogress = function(e) {
    if (e.lengthComputable) {
      const pct = (e.loaded / e.total) * 100;
      console.log('上传进度', pct);
    }
  };
  xhr.onload = function() { console.log('完成', xhr.responseText); };
  const fd = new FormData(); fd.append('tab','book'); fd.append('file', file);
  xhr.send(fd);
  ```
- 实用提示:
  - 不要手动设置 `Content-Type`（浏览器会为 multipart 设置边界）；若手动设置，会导致上传失败或预检出错。
  - 若需要分片上传（设备端有内存/时间限制），需在应用层实现分块并在服务器端提供合并接口（当前固件未实现分片合并）。

---

**3) 下载文件 — `/download`**
- 方法: GET
- 参数: `path` (必需) — 例如 `/book/somebook.txt` 或 `/font/xxx`
- 返回: 文件流，带适当 `Content-Type` 与 `Content-Disposition: attachment; filename="..."`
- curl 示例:
  ```bash
  curl "http://192.168.4.1/download?path=/book/book.txt" -o book.txt
  ```
- 前端 Fetch 示例:
  ```js
  const res = await fetch('http://192.168.4.1/download?path=/book/book.txt');
  if (res.ok) {
    const blob = await res.blob();
    // 可创建链接供用户下载
    const url = URL.createObjectURL(blob);
    // ...
  }
  ```

---

**4) 删除文件 — `/delete`**
- 方法: GET
- 参数: `path` (必需)
- 返回: JSON {"ok":true,"message":"File deleted successfully"} 或错误信息
- 保护机制: 若尝试删除当前正在阅读的书，服务器会返回 400 并拒绝删除（以避免运行时引用失效）。
- curl 示例:
  ```bash
  curl "http://192.168.4.1/delete?path=/book/book.txt"
  ```

---

**5) 时间同步 — `/sync_time`**
- 方法: POST
- Body: 文本（服务器实现会解析 `timestamp` 字段），也可发送 JSON/文本包含 `timestamp` 和 `tzOffsetMinutes` 字段
- 功能: 调用 `settimeofday` 同步设备时间，返回本地时间的可读字符串
- curl 示例:
  ```bash
  curl -X POST -d '{"timestamp": 1630000000}' http://192.168.4.1/sync_time
  ```
- 注意: 服务端也支持 `OPTIONS` 以通过预检。

---

**6) 健康检查 / 版本信息 — `/heartbeat`**
- 方法: GET
- 返回: JSON {"status":"ok","hw":"...","firmware":"...","version":"..."}
- 使用场景: Web 前端在页面加载时可 ping 此接口以判断设备是否已就绪并显示版本信息
- curl 示例:
  ```bash
  curl "http://192.168.4.1/heartbeat"
  ```

---

**错误与状态码（常见）**
- 200: 成功（对于部分操作返回 JSON 或文件流）
- 204: 无内容（用于 OPTIONS 预检）
- 400: 客户端错误（如缺失参数、非法 path、尝试删除当前阅读书等）
- 404: 资源未找到（例如下载路径无效）
- 413: 上传文件过大（服务器可能返回此状态）
- 500/507: 服务器错误或资源不足（内存/存储不足） — 前端应显示用户友好错误并允许重试或稍后重试

**前端最佳实践**
- 在发起上传前，检测文件大小并在客户端给出用户提示，避免尝试上传明显过大的文件。
- 使用 `XMLHttpRequest` 追踪上传进度（`xhr.upload.onprogress`），为用户显示上传条。
- 处理 CORS/OPTIONS：浏览器会自动在需要时发送预检，不要为 `FormData` 手动设置 `Content-Type`。
- 对返回的 JSON 做容错处理：`/list` 可能返回数组或分页对象，检查 `Array.isArray()` 或 `data.files`。
- 对低内存或 SD 不足错误做好友好提示（服务器会返回 5xx/507），并建议用户释放空间或重启设备。
