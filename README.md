# tms-ffmpeg-rest

`tms-koa`插件，增强文件服务，通过 rest 接口操作`ffmpeg`，处理文件服务管理的本地文件。

# 安装

在使用`tms-koa`框架的项目中安装。

> cnpm i tms-koa-ffmpeg

# 运行 demo

> cd demo

## 安装并启动

本地运行。需要在本地安装 ffmpeg。

> cnmp i

> node demo/server

或在 docker 中运行。

> docker-compose up

检查是否正常运行

> curl "http://localhost:3000/ffmpeg/test"

## 准备媒体文件

在`files/upload`目录下放一个`mp4`格式文件，例如：123.mp4。

## 启动 RTP 接收端

可以使用`vlc`播放器作为 RTP 接收端。

参照如下格式新建 sdp 文件，用`vlc`打开。

```
v=0
o=- 0 0 IN IP4 127.0.0.1
s=No Name
c=IN IP4 127.0.0.1
t=0 0
a=tool:libavformat 58.26.101
m=video 5014 RTP/AVP 96
b=AS:200
a=rtpmap:96 MP4V-ES/90000
a=fmtp:96 profile-level-id=1
```

## 播放文件

> curl "http://localhost:3000/ffmpeg/rtp/file/play?path=123.mp4&address=127.0.0.1&vport=5014"

# API 说明

用 RTP 包将指定媒体发送到指定接收位置。

## 播放测试流

> curl "http://localhost:3000/ffmpeg/rtp/test/play?address=&vport=&aport="

| 参数    | 说明              |
| ------- | ----------------- |
| address | 接收 rtp 包的地址 |
| vport   | 接收视频包的端口  |
| aport   | 接收音频包的端口  |

返回命令 id

## 播放文件

> curl "http://localhost:3000/ffmpeg/rtp/file/play?path=&address=&vport=&aport="

| 参数    | 说明                                          |
| ------- | --------------------------------------------- |
| path    | 指定媒体文件路径（参考 tms-koa 文件管理服务） |
| address | 接收 rtp 包的地址                             |
| vport   | 接收视频包的端口                              |
| aport   | 接收音频包的端口                              |

## 停止播放

> curl "http://localhost:3003/ffmpeg/rtp/test/stop?cid="

> curl "http://localhost:3003/ffmpeg/rtp/file/stop?cid="

| 参数 | 说明    |
| ---- | ------- |
| cid  | 命令 Id |

## 暂停播放

> curl "http://localhost:3003/ffmpeg/rtp/test/pause?cid="

> curl "http://localhost:3003/ffmpeg/rtp/file/pause?cid="

| 参数 | 说明    |
| ---- | ------- |
| cid  | 命令 Id |

## 恢复播放

> curl "http://localhost:3003/ffmpeg/rtp/test/resume?cid="

> curl "http://localhost:3003/ffmpeg/rtp/file/resume?cid="

| 参数 | 说明    |
| ---- | ------- |
| cid  | 命令 Id |
