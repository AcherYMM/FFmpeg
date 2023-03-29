# ffmpeg处理流程图

![](D:\git\FFmpeg\pic\2023-03-23-16-53-12-image.png)

# ffmpeg命令分类查询

 ![](D:\git\FFmpeg\pic\2023-03-23-10-29-08-image.png)

查看具体分类所支持的参数：ffmpeg -h type=name

eg:ffmpeg -h muxer=flv

# ffplay播放控制

![](D:\学习\2023-03-23-10-55-46-image.png)

播放一个视频：ffplay -volume

eg:ffplay -volume 10 av1.mp4

# ffmpeg命令选项

![](D:\学习\2023-03-23-11-05-30-image.png)

宽高：ffplay -volume 10 -x 800 -y 480 av1.mp4

从第10秒开始播放：ffplay -volume 10 -x 800 -y 480 -ss 00:00:10 av1.mp4

从第10秒开始播放10s：ffplay -volume 10 -x 800 -y 480 -ss 00:00:10 -t 10 av1.mp4

![](D:\学习\2023-03-23-11-13-26-image.png)

![](D:\学习\2023-03-23-11-15-06-image.png)

![](D:\学习\2023-03-23-11-44-29-image.png)

![](D:\学习\2023-03-23-11-53-02-image.png)

# ffplay播放命令

![](D:\学习\2023-03-23-12-26-41-image.png)

重点：

![](D:\学习\2023-03-23-12-33-27-image.png)

# ffplay简单过滤器

![](D:\学习\2023-03-23-12-42-03-image.png)

更多过滤器：www.ffmpeg.org/ffmpeg-filters.html

# ffmpeg命令参数说明

  ![](D:\学习\2023-03-23-13-10-08-image.png)

# ffmpeg命令提取音视频数据

![](D:\学习\2023-03-23-13-28-03-image.png)

# ffmpeg命令提取像素格式

![](D:\学习\2023-03-23-13-29-15-image.png)

# ffmpeg命令提取PCM数据

![](D:\学习\2023-03-23-16-51-58-image.png)

.wav格式有头部数据，.pcm数据是裸数据。

# ffmpeg命令转封装

![](D:\学习\2023-03-23-17-09-30-image.png)

修改视频码率不改变音频则加上 -acodec copy

ffmpeg -i av1.mp4 -b 400k -acodec copy output_b1.mkv

![](D:\学习\2023-03-23-17-18-33-image.png)

# ffmpeg明令裁剪和合并视频

![](D:\学习\2023-03-23-22-55-31-image.png)

![](D:\学习\2023-03-23-22-56-05-image.png)

测试不同编码拼接：

![](D:\学习\2023-03-23-22-57-09-image.png)

测试结果：

![](D:\学习\2023-03-23-22-57-24-image.png)

#### 注意：拼接时把视频封装格式统一为ts流，拼接输出时再输出你需要的封装格式，比如MP4.

# ffmpeg命令图片和视频转换

![](D:\学习\2023-03-23-23-22-55-image.png)

图片转为视频图片命名格式要相同！

![](D:\学习\2023-03-23-23-23-29-image.png)

# ffmpeg命令录制视频

### 我的电脑录制设备：

查询方法：ffmpeg -list_devices true -f dshow -i dummy

![](D:\学习\2023-03-23-23-43-53-image.png)

摄像头：无

桌面录制设备："screen-capture-recorder"

耳机麦克风："麦克风 (Realtek(R) Audio)"

电脑麦克风："麦克风阵列 (Realtek(R) Audio)"

系统声录制设备："virtual-audio-capturer"

![](D:\学习\2023-03-24-00-14-50-image.png)

复杂过滤器模式：

系统+麦克风声音

ffmpeg -f dshow -i audio="麦克风 (Realtek(R) Audio)" -f dshow -i audio="virtual-audio-capturer" -filter_complex amix=inputs=2:duration=first:dropout_transition=2 a-out2.aac

屏幕录制+麦克风：

ffmpeg -f dshow -i audio="麦克风 (Realtek(R) Audio)" -f dshow -i audio="virtual-audio-capturer" -filter_complex amix=inputs=2:dropout_transition=2 -f dshow -i video="screen-capture-recorder" -y av-out.flv

### 查看视频录制可选参数

视频命令：ffmpeg -f dshow -list_options true -i video="screen-capture-recorder"

![](D:\学习\2023-03-24-00-20-06-image.png)

音频命令：ffmpeg -f dshow -list_options true -i audio="virtual-audio-capturer"

![](D:\学习\2023-03-24-00-22-23-image.png)

！！！

设置参数提升视频质量并减少cpu占用率：

ffmpeg -f dshow -i audio="麦克风 (Realtek(R) Audio)" -f dshow -i audio="virtual-audio-capturer" -filter_complex amix=inputs=2:dropout_transition=2 -f dshow -video_size 1920x1080 -framerate 15 -pixel_format yuv420p -i video="screen-capture-recorder" -vcodec h264_qsv -b:v 3M -y av-out.flv

注释：第一个“-f dshow”是一路输入第二个“-f dshow”又是一路输入，通过”-filter_complex“

mix在一起；最后一个“-f dshow”是视频流，可在其后设置参数：-video_size 1920x1080（分辨率）、-framerate 15（帧率）、-pixel_format yuv420p（像素格式）。

# ffmpeg拉流

咪咕视频·PP体育1:http://39.134.65.162/PLTV/88888888/224/3221225611/index.m3u8

![](D:\学习\2023-03-24-01-28-01-image.png)

# ffmpeg过滤器

### 1、裁剪

ffplay -i av1.mp4 -vf crop=iw/3:ih:iw/3*1:0、

crop=输入宽：输入高：x轴起始位置：y轴起始位置

### 1.5、ffmpeg滤镜内置变量

![](D:\学习\2023-03-24-12-15-32-image.png)

### 2、水印

![](D:\学习\2023-03-24-12-18-37-image.png)

基本功能

![](D:\学习\2023-03-24-12-32-01-image.png)

ffplay -i av1.mp4 -volume 10 -x 600 -y 480 -vf "drawtext=fontsize=100:fontfile=FreeSerif.ttf:text='hello world':fontcolor=green:x=400:y=200:alpha=0.3:box=1:boxcolor=yellow"

特殊用法：

![](D:\学习\2023-03-24-12-38-18-image.png)

![](D:\学习\2023-03-24-12-41-50-image.png)

ffplay -i av1.mp4 -volume 10 -x 600 -y 480 -vf "drawtext=fontsize=100:fontfile=FreeSerif.ttf:text='hello world':fontcolor=green:x=mod(50*t\,w):y=abs(sin(t))*h*0.7:alpha=0.3:box=1:boxcolor=yellow:enable=lt(mod(t\,3)\,1)" 

### 3、图片水印

命令：movie和overlay

![](D:\学习\2023-03-24-12-52-20-image.png)

![](D:\学习\2023-03-24-12-56-07-image.png)

![](D:\学习\2023-03-24-13-09-45-image.png)

ffplay -i av1.mp4 -volume 10 -x 600 -y 480 -vf "movie=logo.jpg[watermark];[in][watermark]overlay=mod(50*t\,main_w):y=abs(sin(t))*h*0.7[out]"

### 4、画中画

![](D:\学习\2023-03-24-13-15-13-image.png)

![](D:\学习\2023-03-24-13-25-35-image.png)

ffplay -i av1.mp4 -vf "movie=av2.mp4,scale=480x270[sub];[in][sub]overlay=20:20:eof_action=2"

### 5、多宫格画面

![](D:\学习\2023-03-24-13-33-51-image.png)

```c++
ffmpeg -i 1.mp4 -i 2.mp4 -i 3.mp4 -i 4.mp4
-filter_complex "nullsrc=size=640x480[base];  //创建基础画布
[0:v]setpts=PTS-STARTPTS,scale=320x240[upperleft];  //第一个流命名为upperleft
[1:v]setpts=PTS-STARTPTS,scale=320x240[upperright]; 
[2:v]setpts=PTS-STARTPTS,scale=320x240[lowerleft]; 
[3:v]setpts=PTS-STARTPTS,scale=320x240[lowerright];  //同上
[base][upperleft]overlay=shortest=1[tmp1];       //将流叠加
[tmp1][upperright]overlay=shortest=1:x=320[tmp2];  //x,y为输出位置
[tmp2][lowerleft]overlay=shortest=1:y=240[tmp3];
[tmp3][lowerright]overlay=shortest=1:x=320:y=240" out.mp4

//完整代码：
ffmpeg -i av1.mp4 -i av1.mp4 -i av1.mp4 -i av1.mp4 -filter_complex "nullsrc=size=640x480[base];[0:v]setpts=PTS-STARTPTS,scale=320x240[upperleft];[1:v]setpts=PTS-STARTPTS,scale=320x240[upperright];[2:v]setpts=PTS-STARTPTS,scale=320x240[lowerleft];[3:v]setpts=PTS-STARTPTS,scale=320x240[lowerright];[base][upperleft]overlay=shortest=1[tmp1];[tmp1][upperright]overlay=shortest=1:x=320[tmp2];[tmp2][lowerleft]overlay=shortest=1:y=240[tmp3];[tmp3][lowerright]overlay=shortest=1:x=320:y=240" out.mp4
```

![](D:\学习\2023-03-24-13-41-24-image.png)
