# ReadPaper

Y.A.P.T.R. 
![Screen](https://github.com/shinemoon/releaseReadPaper/blob/main/pics/cover.png)

## 简介
Yet Another PaperS3 Text Reader: 出于个人趣味的一个读书固件。

- 双击解锁
- 左右翻页
- 点击中心区域菜单

## 版本更新

V1.2:	    繁简转换支持 / 部分问题的解决和修正
V1.1:		可以按照最近打开来浏览书籍/增强了书籍索引的稳定性/内存消耗优化/网页上传优化和稳定性增强
V1.0.2 Alpha:	字体和索引相关逻辑优化
V1.0.1 Alpha: 	索引冲突的稳定性增强及部分操作优化

## 字体壁纸相关

目前也是通过wifi热点传输;

### 字体

- 放置在/font目录下
- 然后在主菜单中选择启用
- 字体文件大小没有限制（不是..)，因为用下面的工具生成的文件size非常稳定在4MB左右……

### 壁纸

- 放置在/image目录下
- 如果该目录下有图片存在，锁屏时会随机选中一张作为锁屏背景。


## 字体生成工具

唯独字体稍微多啰嗦两句：

为了效率和容纳更多的生僻字，使用了进一步压缩的bin文件字体，生成工具和部分字体可以在字体生成工具目录里找到。下载之后，解压7z，有GUI界面帮助生成，需要自行调整字体大小和白色阈值。也有几个已经生成的字体供下载和测试。

https://github.com/shinemoon/releaseReadPaper

**文件路径避免使用中文!!!**

![GenerateTool](https://github.com/shinemoon/releaseReadPaper/blob/main/pics/generateTool.png)

总体来讲，白色阈值越高，字体越细，但是此时会出现笔画发虚或者坑坑洼洼的情况，所以必须承认这也是压缩带来的损失，使得字体效果是有折扣的，为了达到较好的显示效果，有时要反复调校一下参数——

![Font](https://github.com/shinemoon/releaseReadPaper/blob/main/pics/fontdemo.png)

所以在工具中增加了预览图片生成的功能，以提高调校效率；而调校的标准大致就是看让笔画完整性和毛刺能到达一个较好的平衡即可。当然不用太紧张于演示图片效果似乎总不够完美——距离产生美，实际显示在远小于显示器图片尺寸的屏幕上时，其实会好很多。

![GeneratePreview](https://github.com/shinemoon/releaseReadPaper/blob/main/pics/fontdemo.jpg)


## 壁纸生成工具

尺寸裁剪调整，灰度化，这些虽然简单，但是毕竟也是个啰嗦的事情，为了方便点，顺便也做了个快速处理壁纸的工具，在壁纸生成工具下。

**文件路径避免使用中文!!!**

![ImageConvertor](https://github.com/shinemoon/releaseReadPaper/blob/main/pics/imageConvertor.png)



## 安装

到M5Stack的烧录程序中搜索**ReadPaper**即可
![M5StackBurn](https://github.com/shinemoon/releaseReadPaper/blob/main/pics/publish.png)


## 图样

锁屏    
![demo](https://github.com/shinemoon/releaseReadPaper/blob/main/pics/demo.jpg)

阅读菜单    
![demoI](https://github.com/shinemoon/releaseReadPaper/blob/main/pics/demoI.jpg)

主菜单    
![demoII](https://github.com/shinemoon/releaseReadPaper/blob/main/pics/demoII.jpg)


## 感谢

不能免俗， 感谢M5Stack的轻巧设计；

也更感谢@梦西游啊游 大佬作为被摸的那块石头的开创之功，他几乎展示了这块开发板作为TEXT阅读器的上限。

题外话，就普通使用来说‘阅读卡片’绝对是一个更平衡和有前途的选择，@梦西游啊游大佬的技术实力和热情都让人佩服，如果不是因为我想借机会摸索一遍开发流程，其实这个轮子是大可不必造的。

希望后面可以有更多的出色的开放硬件和固件可供大家把玩，有问题也可以到Github的issue来讨论。


