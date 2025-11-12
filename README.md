# ReadPaper

Y.A.P.T.R. 
![Screen](https://github.com/shinemoon/releaseReadPaper/blob/main/pics/cover.png)

## 简介
Yet Another PaperS3 Text Reader: 出于个人趣味的一个读书固件。

- 双击解锁
- 左右翻页
- 点击中心区域菜单
- AllinOne的浏览器扩展管理工具

## 版本更新
V1.3:       基于浏览器扩展管理工具上线/ API / 索引逻辑优化 ；
V1.2.10:	深色模式支持/壁纸工具优化，调整壁纸加载逻辑；/支持竖排，调整字体压缩方式并更新了字体生成工具（**此版本之前生成的字体建议利用工具重新生成或重新从本仓库下载，才能达到最佳效果**），调整锁屏样式/ 自适应旋转；
V1.2:	    繁简转换支持 / 部分问题的解决和修正；
V1.1:		可以按照最近打开来浏览书籍/增强了书籍索引的稳定性/内存消耗优化/网页上传优化和稳定性增强；

## 客户端工具

![GUI](https://github.com/shinemoon/releaseReadPaper/blob/main/pics/ExtCover.png)

从V1.3起，除了原有的浏览器网页访问，开始支持跨平台的浏览器扩展，用来进行文件管理、字体生成、公告发布以及未来其他扩展功能 (原有网页功能依然可以使用但是不会继续扩展）

扩展获取： 

[ReadPaper Utils@Google Chrome (梯子）](https://chromewebstore.google.com/detail/readpaper-utils/fgkgjmmeoaojnhkeiebbibgbodikmgoe)    
[ReadPaper Utils@MicroSoft Edge](https://microsoftedge.microsoft.com/addons/detail/readpaper-utils/ablggfkhbegbnlgjbleoklekinmglnia)    
ReadPaper Utils@Firefox: 审查中   

安卓移动端：在Firefox上架前，推荐狐猴浏览器然后从内嵌Chrome或者Edge市场安装.


### 字体

- 放置在/font目录下
- 然后在主菜单中选择启用
- 字体文件大小没有限制（不是..)，因为用下面的工具生成的文件size最大4MB左右，通常在2MB以内
- 注意字体切换会导致书籍（如果当前索引不是在所切换的字体下完成的）索引强制重新开始并且跳转到第一页
- 从扩展界面进行字体压缩调整以及生成 (老的打包程序依然保留可以使用，但是类似的，后续可能不再更新）

![GUI](https://github.com/shinemoon/releaseReadPaper/blob/main/pics/Font.png)


为了效率和容纳更多的生僻字，使用了进一步压缩的bin文件字体，生成工具和部分字体可以在字体生成工具目录里找到。下载之后，解压7z，有GUI界面帮助生成，需要自行调整字体大小和白色阈值。也有几个已经生成的字体供下载和测试。


![Font](https://github.com/shinemoon/releaseReadPaper/blob/main/pics/fontdemo.png)


### 壁纸

- 放置在/image目录下
- 如果该目录下有图片存在，优先设定当前书籍同名壁纸，然后是default.png，如果都没有，则使用系统自带壁纸（不再用随机壁纸）。


## 安装

到M5Stack的烧录程序中搜索**ReadPaper**即可
![M5StackBurn](https://github.com/shinemoon/releaseReadPaper/blob/main/pics/publish.png)


## 图样

锁屏    
![demo](https://github.com/shinemoon/releaseReadPaper/blob/main/pics/demo.jpg)
![demo](https://github.com/shinemoon/releaseReadPaper/blob/main/pics/demo3.jpg)

竖排阅读

![demo](https://github.com/shinemoon/releaseReadPaper/blob/main/pics/demo2.jpg)

深色模式
![demo](https://github.com/shinemoon/releaseReadPaper/blob/main/pics/demo4.jpg)

阅读菜单    
![demoI](https://github.com/shinemoon/releaseReadPaper/blob/main/pics/demoI.jpg)

主菜单    
![demoII](https://github.com/shinemoon/releaseReadPaper/blob/main/pics/demoII.jpg)


## 感谢

不能免俗， 感谢M5Stack的轻巧设计；

也更感谢@梦西游啊游 大佬作为被摸的那块石头的开创之功，他几乎展示了这块开发板作为TEXT阅读器的上限。

题外话，就普通使用来说‘阅读卡片’绝对是一个更平衡和有前途的选择，@梦西游啊游大佬的技术实力和热情都让人佩服，如果不是因为我想借机会摸索一遍开发流程，其实这个轮子是大可不必造的。

希望后面可以有更多的出色的开放硬件和固件可供大家把玩，有问题也可以到Github的issue来讨论。

