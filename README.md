# Python-FFmpeg-RTSP-Sound recording
Python使用FFmpeg对RTSP进行音频拉流并录音

最近有项目需要在树莓派上进行录音，由于树莓派3B自身对麦克风录音支持不足，刚好前阵子破解了小蚁摄像头1080p国际版，其中有一个功能是RTSP服务器，正好利用RTSP的音频流进行拉流并保存录音。

Python使用FFmpeg对RTSP进行音频拉流，应该是最好的选择。因为FFmpeg开源，且对音频的解码支持十分广泛。FFmpeg的协议文档在这里：https://ffmpeg.org/ffmpeg-protocols.html#Examples

使用环境
python版本：python3
PC系统：windows10

具体实现方法如下：
1、小蚁破解后的WEB界面有RTSP地址，如下图：

2、Python安装FFmpeg-python模块

pip install ffmpeg-python

3、下载win10可用的FFmpeg：http://ffmpeg.org/  ，并放在脚本相同的目录下，我之前有下载了一个可用，链接：https://pan.baidu.com/s/18qIlvpyP1mpVvzc9kFzz4g ，提取码：egud 

 
4、python代码：

# -*- coding:UTF-8 _*-
import ffmpeg
 
host = '192.168.50.166/ch0_2.h264'
 
#子进程
(
    ffmpeg
        .input('rtsp://' + host, allowed_media_types='audio', rtsp_transport='tcp')['a']  # allowed_media_types='audio' 只读取音频流
        .filter('volume', 5)  # 音量大小控制
        .output('saved_audio.aac', ac=1, ar='16k')  # ac是声道，ar是采样率
        .overwrite_output()
        .run(capture_stdout=True)
)
