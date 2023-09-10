## ffmpeg 添加字幕

测试 视频流+音频流+字幕：`ffmpeg -i 纯视频流.mp4 -i 纯音频流.wav -vf subtitles=测试字幕.ass 输出视频.mp4`

## ffmpeg 提取并转换音频

> for video in *.mp4; do ffmpeg -i "$video" -vn -acodec libvorbis "${video%.mp4}.ogg"; done

## ffmpeg 提取并复制音频

> for video in *.mp4; do ffmpeg -i "$video" -vn -c:a copy "${video%.mp4}.aac"; done

## 批量下载b站视频

> for i in {1..5}; do /home/litchi/Apps/you-get/you-get https://www.bilibili.com/video/BV1o5411d77L?p=$i -c /home/litchi/Apps/cookies.txt; done 
