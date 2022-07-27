---
title: ffmpeg将mp4切成m3u8
date: 2022-07-27 10:19:45
tags:
description:
categories:
---

## 背景

需要将mp4视频文件加密成m3u8文件给客户端，防止盗用

## 环境

OpenSSL

ffmepg



## 原理

利用openSSL生成加密文件key，用ffmepg将mp4文件切割成m3u8文件（包含key信息）和多个ts视频文件

## 使用

安装ffmepg

### 使用OpenSSL生成秘钥

> openssl rand 16 > [秘钥存放位置名称]

>  例：openssl rand 16 > D:\video\encrypt.key



### 生成IV

> openssl rand -hex 16

> 生成一串字符串：f169891eb22d3926af868e898ce64eaa

### 编写keyinfo文件

```
https://XXX.cos.ap-shanghai.myqcloud.com/XXX.key
D:\video\encrypt.key
f169891eb22d3926af868e898ce64eaa
```

> 一共有三段信息
>
> 第一段：解密文件路径，必须是URI，用来解密视频文件(放在了腾讯云上，写入到m3u8文件里)
>
> 第二段：是加密文件路径（放在了本地，之后命令用到）
>
> 第三段：之前生成的IV



### 用ffmpeg 将视频切片（不加密）

> ffmpeg -y -i d:\test.mp4 -c:v libx264 -hls_time 5 -hls_playlist_type vod -hls_segment_filename d:\video\%06d.ts  d:\video\test.m3u8

### 用ffmpeg 将视频切片（加密）

> ffmpeg -y -i d:\test.mp4 -c:v libx264 -hls_time 5 -hls_key_info_file D:\video2\encrypt.keyinfo  -hls_playlist_type vod -hls_segment_filename d:\video2\%06d.ts  d:\video2\test.m3u8

> ffmpeg -y -i D:\openssl_key\test.mp4 -c:v libx264 -c:a copy -f hls -hls_time 180 -hls_list_size 0 -hls_key_info_file D:\openssl_key\enc.keyinfo -hls_playlist_type vod -hls_segment_filename D:\openssl_key\file%d.ts D:\openssl_key\playlist.m3u8

| 命令参数                | 解释                                         |
| ---------------------- | ------------------------------------------- |
| -y                     | 不经过确认，输出时直接覆盖同名文件。                         |
| -c                     | 指定编码器                                                   |
| -c copy                | 直接复制，不经过重新编码（这样比较快）                       |
| -i                     | 指定输入文件                                                 |
| -title                 | 设置标题                                                     |
| -author                | 设置作者                                                     |
| -copyright             | 设置版权                                                     |
| -f                     | 强制设置输入输出的文件格式，默认情况下ffmpeg会根据文件后缀名判断文件格式 |
| -hls_key_info_file     | keyinfo文件路径                                              |
| -hls_time              | 每段文件的时间长度(单位:秒)                                  |
| -hls_list_size 0       | 索引播放列表的最大列数 默认5，0 为不限制                     |
| -hls_playlist_type vod | 表示当前的视频流并不是一个直播流，而是点播流                 |
| -hls_segment_filename  | 输出 ts和m3u8 文件路径中间空格 ，例如：D:\openssl_key\ file%d.ts D:\openssl_key\playlist.m3u8 |
| %06d                   | 表示6位数字，从000000开始                                    |

### 最终生成的文件

![01.png](01.png)

m3u8文件：

![02.png](02.png)

```
#EXTM3U                    M3U8文件头，必须放在第一行;
#EXT-X-MEDIA-SEQUENCE      第一个TS分片的序列号，一般情况下是0，但是在直播场景下，这个序列号标识直播段的起始位置; #EXT-X-MEDIA-SEQUENCE:0
#EXT-X-TARGETDURATION      每个分片TS的最大的时长;   
#EXT-X-TARGETDURATION:10     每个分片的最大时长是 10s
#EXT-X-ALLOW-CACHE         是否允许cache;          
#EXT-X-ALLOW-CACHE:YES      
#EXT-X-ALLOW-CACHE:NO    默认情况下是YES
#EXT-X-ENDLIST             M3U8文件结束符；
#EXTINF                    extra info，分片TS的信息，如时长，带宽等；一般情况下是    
#EXTINF:<duration>,[<title>] 后面可以跟着其他的信息，逗号之前是当前分片的ts时长，分片时长 移动要小于 
#EXT-X-TARGETDURATION 定义的值；
#EXT-X-VERSION             M3U8版本号
#EXT-X-DISCONTINUITY       该标签表明其前一个切片与下一个切片之间存在中断。下面会详解
#EXT-X-PLAYLIST-TYPE       表明流媒体类型；
#EXT-X-KEY                 是否加密解析，    
#EXT-X-KEY:METHOD=AES-128,URI="https://priv.example.com/key.php?r=52"    加密方式是AES-128,秘钥需要请求   https://priv.example.com/key.php?r=52  ，请求回来存储在本地；
```



### 播放

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta http-equiv="content-type" content="text/html; charset=utf-8" />
    <title>视频播放</title>
    <link href="./videojs/video-js.css" rel="stylesheet">
</head>
<body>
<video id=example-video width=800 height=600 class="video-js vjs-default-skin vjs-big-play-centered" controls poster="D:test.jpg">
    <source
            src="./my/test.m3u8"
            type="application/x-mpegURL">
</video>
<input type="button" onClick="switchvideo()" value="switch"/>

<script src="./videojs/video.js"></script>
<script src="./videojs/videojs-contrib-hls.js"></script>
<script>
    var player = videojs('example-video');
    //player.play();

    function switchvideo(){
        player.src({
            src: './my/test.m3u8',
            type: 'application/x-mpegURL',
            withCredentials: true
        });
        player.play();
    }
</script>

</body>
</html>
```

文件目录

![03.png](03.png)







[windows 安装openssl](https://link.juejin.cn/?target=https%3A%2F%2Fwww.cnblogs.com%2Fdshvv%2Fp%2F12271280.html)

[windows 安装 ffmpeg](https://link.juejin.cn/?target=https%3A%2F%2Fblog.csdn.net%2Fqq_43803367%2Farticle%2Fdetails%2F110308401)

[参考链接1](https://juejin.cn/post/6959825909763276831)

[参考链接2](https://blog.csdn.net/Sunshine_Moon/article/details/102524173)