---
description: MediaInfo的排版，有微小出入亦可接受
---

# PART 3 MediaINFO

使用**Courier New字体**对齐内容，每一项小标题全部大写，统一使用英文逗号，大小写规范如下

{% code title="MediaInfo排版规范" %}
```
RELEASE.NAME........: Sword.Art.Online.the.Movie.Progressive.Aria.of.a.Starless.Night.2021.2160p.HEVC.10bit.TrueHD7.1.keprice@SRVFI-Raws.mkv
RELEASE.DATE........: 2022-07-09                   //日期 格式xxxx-xx-xx
RELEASE.SIZE........: 114514.1 GiB                 //文件大小, 以下除帧率保留三位外，均保留一位有效数字, 所有的数字与单位之间均空一格
RELEASE.FORMAT......: Matroska                     //容器格式, 如 Matroska, MPEG-4 Part 14
OVERALL.BITRATE.....: 1919.8 Mb/s                  //总码率
RESOLUTION..........: 3840x2160                    //分辨率
BIT.DEPTH...........: 10 bits                      //位深
FRAME.RATE..........: 60.000 FPS                   //帧率
VIDEO...............: HEVC, Main@L5@Main           //Format, Format profile --以下均为每一项后写一个逗号，后空一格，再写下一项
AUDIO#01............: Chinese, 8 channels, E-AC-3  //语言, n 声道, Format
AUDIO#02............: Engilsh, 2 channels, AAC     
SUBTITLE#01.........: CHS, PGS                     //语言代码, Format --常用如下 CHS>简体中文 CHT>繁体中文 ENG>英语 JPN>霓虹语 KOR>棒语 DEU>德语 FRA>法语 RUS>毛语
SUBTITLE#02.........: CHT, ASS
SUBTITLE#03.........: CHT, SRT
SUBTITLE#04.........: CHT&ENG, ASS                 //语言1&语言2, Format --双字幕则按上下字幕顺序填写，示例为繁体中文上英文下
SUBTITLE#05.........: ENG&CHT, ASS
SOURCE..............: Encode                       //资源类型，如 WEB-DL Encode 等
UPLOADER............: Keprice@SRVFI-Raws           //上传者@组织 up.Name@xx-Raws
```
{% endcode %}

### _示例_

**效果如下**

![03eg](../.gitbook/assets/part\_03eg.PNG)

```
[quote][font=Courier New][url=https://sm.ms/image/Pq7IXvwe8FTruoK][img]https://s2.loli.net/2022/07/10/Pq7IXvwe8FTruoK.png[/img][/url]
RELEASE.NAME........: Sword.Art.Online.the.Movie.Progressive.Aria.of.a.Starless.Night.2021.2160p.HEVC.10bit.TrueHD7.1.keprice@SRVFI-Raws.mkv
RELEASE.DATE........: 2022-07-09 
RELEASE.SIZE........: 26.4 GiB
RELEASE.FORMAT......: Matroska
OVERALL.BITRATE.....: 39.1 Mb/s
RESOLUTION..........: 3840x2160
BIT.DEPTH...........: 10 bits
FRAME.RATE..........: 23.976 FPS
VIDEO...............: HEVC, Main 10@L5@Main
AUDIO#01............: Japanese, 2 channels, PCM
AUDIO#02............: Japanese, 8 channels, MLP FBA 16-ch
AUDIO#03............: Japanese, 6 channels, AC-3
AUDIO#04............: Japanese, 6 channels, MLP FBA
AUDIO#05............: Japanese, 6 channels, AC-3
AUDIO#06............: Japanese, 2 channels, PCM
AUDIO#07............: Japanese, 2 channels, AC-3
AUDIO#08............: Japanese, 2 channels, DTS XLL
SUBTITLE#01.........: JPN, PGS 
SUBTITLE#02.........: ENG, PGS
SOURCE..............: Encode
UPLOADER............: Keprice@SRVFI-Raws
[/font][/quote]
```
