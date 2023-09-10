## ffmpeg 添加字幕

测试 视频流+音频流+字幕：`ffmpeg -i 纯视频流.mp4 -i 纯音频流.wav -vf subtitles=测试字幕.ass 输出视频.mp4`

## ffmpeg 提取并转换音频

> for video in *.mp4; do ffmpeg -i "$video" -vn -acodec libvorbis "${video%.mp4}.ogg"; done

## ffmpeg 提取并复制音频

格式常为 aac 或 m4a，可通过 VLC 的“工具——编解码器信息”查看

> for video in *.mp4; do ffmpeg -i "$video" -vn -c:a copy "${video%.mp4}.aac"; done

## 批量下载b站视频

> for i in {1..5}; do you-get https://www.bilibili.com/video/BVxxxxxxxxxxx?p=$i -c cookies.txt; done 
