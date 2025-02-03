# 格式转换：



## 音频转换格式: 

除了视频转换，ffmpeg还可以做音频、图片转换

`ffmpeg -i sound.wav sound.mp3` (把sound的格式从wav转成mp3)

## 视频转换格式：

`ffmpeg -i vedio.avi vedio.mp4` (把vedio的格式从avi转成mp4)

ffmpeg 对于mp4默认选择h264编码方式；可以 `-c:v [vedio encoder]` 手动指定一个视频编码器;`libx264`是ffmpeg默认提供的h264视频编码器，`h264_nvenc`是用英伟达显卡硬件加速

例如：
`ffmpeg -i vedio.avi -c:v libx264 vedio.mp4` （生成文件的时间会慢一点）
`ffmpeg -i vedio.avi -c:v h264_nvenc vedio.mp4`  （会更快）

## 通过设置参数压缩视频文件的大小：

**对于libx264编码器：**

### 可以设置 `-preset [值]` 

`ffmpeg -i vedio.avi -c:v libx264 -preset veryfast vedio.mp4`

值可以是：

![image-20250127005113640](https://abigail-1315839746.cos.ap-nanjing.myqcloud.com/typora/image-20250127005113640.png)

`ultrafast` 编码速度最快，但是产生的文件较大；`veryslow` 反之

可以用 `varyfast`

### 还可以设置`-crf [值]`

`ffmpeg -i vedio.avi -c:v libx264 -crf 22 vedio.mp4`

crf用来控制图片质量，取值范围在 `0 - 51` ;

0质量最高是无损，输出的文件较大，51质量最差，但是输出的文件最小

通常用到的范围是 `19 - 28`

# 视频过滤器

对视频中的图像进行变换，比如修改尺寸、裁剪、添加滤镜等

官方文档中有上百种过滤器，用法都一样： `-vf "某种过滤器的名字=值1:值2(多个值之间用冒号分隔),(多个过滤器用逗号分隔)另一种过滤器的名字=值"` 

可以设置一组过滤器 like ↓；前一个过滤器的输出作为下一个过滤器的输入；比如先把视频图片旋转90°，再给它添加一个滤镜

![image-20250127010307501](https://abigail-1315839746.cos.ap-nanjing.myqcloud.com/typora/image-20250127010307501.png)

## scale修改视频尺寸：

`ffmpeg -i vedio.avi -c:v libx264 -vf "scale=1024:576" vedio.mp4` (表示把视频放缩到1024长和576宽)

`ffmpeg -i vedio.avi -c:v libx264 -vf "scale=-1:720" vedio.mp4` (可将冒号任意一边设置成-1，让ffmpeg根据另一个自动推算；本例是将视频变成720p)

## transpose旋转视频：

`ffmpeg -i vedio.avi -c:v libx264 -vf "transpose=2" vedio.mp4` （值为2表示把视频逆时针旋转90°）

其它数字的含义请看官网：

![image-20250127011634324](https://abigail-1315839746.cos.ap-nanjing.myqcloud.com/typora/image-20250127011634324.png)

## crop裁剪视频中的某一块区域：

`ffmpeg -i vedio.avi -c:v libx264 -vf "crop=400:400:100:100" vedio.mp4` （把视频这块区域裁掉）

其中，`crop=w:h:x:y`  分别表示区域的宽和高还有左上角的坐标

`ffmpeg -i vedio.avi -c:v libx264 -vf "crop=iw/3:ih/3" vedio.mp4` (让ffmpeg自己算，`iw`是input width，表示输入视频的宽度，`ih`则表示输入视频的高度)

## 组合这些过滤器：

`ffmpeg -i vedio.avi -c:v libx264 -vf "scale=-1:720,transpose=2" vedio.mp4` （缩放成720p后再逆时针旋转90°）

# 剪切与合并

## 剪切掉某一段视频：

`ffmpeg -i test.avi -c:v libx264 -ss 00:00:03 -t 00:00:05 output.mp4` (把从视频的3s开始，后5s的这一段视频裁掉)

`-ss` 表示起始位置，`-t `表示持续的时间；时间的格式是时分秒，`hh:mm:ss`

`-ss` 需要放在 -i 的后面才能做到分毫不差的剪切

`ffmpeg -i test.avi -c:v libx264 -ss 00:00:03 -to 00:00:08 output.mp4` (把视频3s到8s这一段视频裁掉)

`-to` 表示终止位置

## 合并视频：

先把所有文件列举在一个`txt`文件中，格式如下

![image-20250127015215977](https://abigail-1315839746.cos.ap-nanjing.myqcloud.com/typora/image-20250127015215977.png)

`ffmpeg -f concat -i mylist.txt -c copy output.mp4` 

`-f concat` 指定接下来要输入的文件是一个视频列表，`-i mylist.txt` 传入了这个列表文件，`-c copy` 表示不希望重新编码，而是复制原始视频的数据（输入的视频格式都是mp4，和最终要输出的视频格式一致，不希望重新编码浪费时间）

# 音频过滤器

命令格式跟视频过滤器很像，`-vf`（vedio filter） 变成了 `-af`（audio filter） 

## volume调节音量：

`ffmpeg -i test.mp4 -af "volume=1.5" output.mp4` 

## loudnorm统一视频的音量：

`ffmpeg -i test.mp4 af "loudnorm=I=-5:LRA=1" output.mp4`

## 添加高通、低通滤波器、甚至均衡器：

`ffmpeg -i test.mp4 -af "equalizer=f=1000:width_type=h:width=200:g=-10" output.mp4`

# -an 删除音频轨

`ffmpeg -i test.mp4 -an output.mp4`

类似的有

![image-20250127020854728](https://abigail-1315839746.cos.ap-nanjing.myqcloud.com/typora/image-20250127020854728.png)

# 实用技巧

## 提取视频缩略图：

`ffmpeg -i test.mp4 -vf "fps=1/10,scale=-2:720" thumbnail-%03d.jpg` 

`fps` 指定了输出文件的帧率，1/10表示每10s输出一张图片；

`scale` 指定了输出图像的大小；

最后是输出的图像的文件名，`thumbnail`- 和 编号 `%03d` 组成，图片文件格式是 `jpg` （ `%03d` 就是C语言打印整数 %d 的变化）

![image-20250127022504363](https://abigail-1315839746.cos.ap-nanjing.myqcloud.com/typora/image-20250127022504363.png)

## 合并视频和音频：

`ffmpeg -i [音频文件] -i [视频文件] -c copy [输出到]` 

例如：

`ffmpeg -i sound.mp3 -i vedio.mp4 -c copy output.mp4` (输入sound和vedio，合并，生成到output中)

## 添加水印

`ffmpeg -i test.mp4 -i cat.jpg -filter_complex "overlay=100:100" output.mp4`

cat.jpg 是水印图片，test.mp4 是要被打上水印的视频 ，用 -i 输入到 ffmpeg 中，然后用 overlay 过滤器把水印图片叠到原始视频上，overlay的值指定了水印叠在视频图像的位置

<img src="https://abigail-1315839746.cos.ap-nanjing.myqcloud.com/typora/image-20250127023413044.png" alt="image-20250127023413044" style="zoom:50%;" />

 ![image-20250127023349983](https://abigail-1315839746.cos.ap-nanjing.myqcloud.com/typora/image-20250127023349983.png)

## Gif动图的转换

