---
description: 坚决不动手
---

# 😅 使用Python快速生成简介

{% hint style="info" %}
该脚本尚未完善，如有bug请向Nang@SRVFI-Raws反馈
{% endhint %}

### 所需依赖

* opencv-python
* pymediainfo
* requests
* json

使用前请先在`if __name__ == "__main__":`里自行修改api等信息

```python
import cv2
import os
import time
import pathlib
import requests
import json
import pymediainfo
import random


class PtGenPlus:
    def __init__(self):
        self.smms_pic_url = "https://sm.ms/api/v2/upload"  # smms图床上传网址
        self.smms_pic_api = "NIx23JsYkdf1145141919810aMkmjp"  # smms图床API
        self.pt_gen_url = "https://ptgen.srvfi.top/"  # ptgen的域名
        self.pt_gen_api = "&apikey=8hFJLiSqGRkHcPAu"  # ptgen的APIkey
        self.bgm_douban_imdb_url = "https://movie.douban.com/subject/27624762"  # bgm，豆瓣，imdb详细url
        self.source_path = "NCOP1.m2ts"  # 原视频地址
        self.encode_path = "NCOP1.mp4"  # Encode视频地址
        self.encode_or_dl = "Encode"  # Encode or WEB-DL or Remux
        self.uploader_name = "Keprice@SRVFI-Raws"  # 上传者

    def final_info_generate(self):
        with open("final_infomation.txt", "w", encoding="utf-8") as final_infomation:
            final_infomation.write(
                "[quote][b][size=3][color=blue]原盘来自U2，感谢原发布者[/color][/size][/b][/quote]\n" +
                "[quote][b][size=3]原盘的画质尚可，主要问题的低分辨率拉伸带来的模糊。" +
                "我们使用基于生成对抗网络的动漫图像超分辨率模型 RealCUGAN-Pro " +
                "(Real Cascade U-Nets Generative Adversarial Networks) 超分辨率至2160P，" +
                "进行了常规的处理，轻微改变了画风\nThe Source quality is relatively acceptable" +
                ", and the major problem is blurring caused by upscaling from low resolution." +
                "We upscaled it to 2160P with RealCUGAN-Pro(AI Super Resolution Model for Anime Images " +
                "Based on Generative Adversarial Networks), treated it with some mild regular processing" +
                ", slightly changed the style.[/size][/b][/quote]\n\n")
            final_infomation.write(self.get_pt_gen_info())
        with open("final_infomation.txt", "a", encoding="utf-8") as final_infomation:
            for i1 in self.get_media_info():
                final_infomation.write(i1)
        with open("final_infomation.txt", "a", encoding="utf-8") as final_infomation:
            for i2 in self.get_screens():
                final_infomation.write(i2)

    def get_pt_gen_info(self):
        pt_gen_response = requests.get(self.pt_gen_url + '?url=' + self.bgm_douban_imdb_url + self.pt_gen_api)
        dict_pt_gen_response = json.loads(pt_gen_response.text)
        return dict_pt_gen_response["format"]

    def get_media_info(self):
        write_info_list = []
        encode_media_info = pymediainfo.MediaInfo.parse(self.encode_path, output="JSON")
        encode_tracks = json.loads(encode_media_info)["media"]["track"]
        write_info_list.append(
            "\n\n" + "[quote][font=Courier New][url=https://sm.ms/image/YrDuWZ91stKFzLl]" +
            "[img]https://s2.loli.net/2022/08/05/YrDuWZ91stKFzLl.png[/img][/url]")
        write_info_list.append(
            "\n" + "RELEASE.NAME........: " +
            str(pathlib.PureWindowsPath(self.encode_path)).split("\\")[-1])
        write_info_list.append(
            "\n" + "RELEASE.DATE........: " +
            time.strftime("%Y-%m-%d", time.localtime()))
        write_info_list.append(
            "\n" + "RELEASE.SIZE........: " +
            encode_tracks[0]["FileSize_String"])
        write_info_list.append(
            "\n" + "RELEASE.FORMAT......: " +
            encode_tracks[0]["Format"])
        write_info_list.append(
            "\n" + "OVERALL.BITRATE.....: " +
            encode_tracks[0]["OverallBitRate_String"])
        # 写视频轨参数
        video_track_id = 0
        for video_track_index, video_track in enumerate(encode_tracks):
            if video_track["@type"] == "Video":
                write_info_list.append(
                    "\n" + "RESOLUTION..........: " +
                    encode_tracks[video_track_index]["Width"] + "x" +
                    encode_tracks[video_track_index]["Height"])
                write_info_list.append(
                    "\n" + "BIT.DEPTH...........: " +
                    encode_tracks[video_track_index]["BitDepth_String"])
                write_info_list.append(
                    "\n" + "FRAME.RATE..........: " +
                    encode_tracks[video_track_index]["FrameRate"] + " FPS")
                write_info_list.append(
                    "\n" + "VIDEO...............: " +
                    encode_tracks[video_track_index]["Format_String"] + ", " +
                    encode_tracks[video_track_index]["Format_Profile"])
                video_track_id += 1
        if video_track_id != 1:
            print("可能存在多条视频轨道或无视频轨，请检查")
        # 写音轨参数
        audio_track_id = 1
        for audio_track_index, audio_track in enumerate(encode_tracks):
            if audio_track["@type"] == "Audio":
                write_info_list.append(
                    "\n" + "AUDIO#" + str(audio_track_id).zfill(2) + "............: ")

                if "Language_String" in audio_track:
                    write_info_list.append(
                        encode_tracks[audio_track_index]["Language_String"])
                else:
                    print("音轨" + str(audio_track_id) + "缺少语言信息，请自行填写")
                    write_info_list.append("Ambiguous!!!")

                write_info_list.append(
                    ", " + encode_tracks[audio_track_index]
                    ["Channels_String"] + ", " + encode_tracks[audio_track_index]["Format_String"])
                audio_track_id += 1
        # 写字幕轨参数
        subtitle_track_id = 1
        for subtitle_track_index, subtitle_track in enumerate(encode_tracks):
            if subtitle_track["@type"] == "Text":
                write_info_list.append(
                    "\n" + "SUBTITLE#" + str(subtitle_track_id).zfill(2) + ".........: ")

                if "Language_String" in subtitle_track and "Title" not in subtitle_track:
                    write_info_list.append(
                        encode_tracks[subtitle_track_index]["Language_String"])
                elif "Title" in subtitle_track:
                    write_info_list.append(
                        encode_tracks[subtitle_track_index]["Title"])
                else:
                    print("字幕轨" + str(subtitle_track_id) + "缺少语言信息，请自行填写")
                    write_info_list.append("Ambiguous!!!")

                write_info_list.append(
                    ", " + encode_tracks[subtitle_track_index]["Format_String"])
                subtitle_track_id += 1
        # print(encode_media_info)
        write_info_list.append(
            "\n" + "SOURCE..............: " + self.encode_or_dl)
        write_info_list.append(
            "\n" + "UPLOADER............: " + self.uploader_name + "\n" + "[/font][/quote]\n\n")

        return write_info_list

    def get_screens(self):
        headers = {'Authorization': self.smms_pic_api}
        url = self.smms_pic_url
        cap_1 = cv2.VideoCapture(self.source_path)
        cap_2 = cv2.VideoCapture(self.encode_path)

        frames_num_1 = int(cap_1.get(7))
        frames_num_2 = int(cap_2.get(7))
        if frames_num_1 != frames_num_2:
            print("视频可能有问题，帧数相差" + str(frames_num_1 - frames_num_2))
            print('Source视频总帧数：' + str(frames_num_1))
            print('Encode视频总帧数：' + str(frames_num_2))

        n = 5  # 这是按间隔取帧的参数，例如这里是5的话会把视频按时间轴从头到尾分为7段，去掉头段和尾段，取中间五段中的随机帧
        split_num = int(frames_num_1 / (n + 2))  # 切分块的帧数
        root_path = os.path.abspath('.')
        output_dir = "%s/compare_pics" % root_path  # 图片保存路径
        output_dir = output_dir + "__" + str(pathlib.PureWindowsPath(self.encode_path)).split("\\")[-1]
        os.makedirs(output_dir, exist_ok=True)

        split_num_deal = split_num
        pic_num = []
        # 开头的固定内容
        pic_num.append(
            '[url=https://sm.ms/image/Ea3jHpRGi76fqr4]' +
            '[img]https://s2.loli.net/2022/08/05/Ea3jHpRGi76fqr4.png[/img][/url]\n')
        pic_num.append(
            '[b] right click on the image and open it in a new tab to see the full-size one [/b]\n')
        pic_num.append(
            '[url=https://sm.ms/image/9iZHPU5hJnFlWIg]' +
            '[img]https://s2.loli.net/2022/07/10/9iZHPU5hJnFlWIg.png[/img][/url]\n')

        for i in range(n):
            print("截图上传中...( ˶º̬˶ )୨⚑...第" + str(i + 1) + "组，共有" + str(n) + "组")
            random_frame = random.randint(int(split_num / (-2)), int(split_num / 2))
            split_num_deal += random_frame

            cap_1.set(cv2.CAP_PROP_POS_FRAMES, split_num_deal)
            cap_2.set(cv2.CAP_PROP_POS_FRAMES, split_num_deal)

            s, s_1 = cap_1.read()
            e, e_1 = cap_2.read()

            path_s = output_dir + "/" + str(pathlib.PureWindowsPath(self.encode_path)).split("\\")[-1] + "__" + \
                     str(split_num_deal) + '_source_' + '.jpg'
            path_e = output_dir + "/" + str(pathlib.PureWindowsPath(self.encode_path)).split("\\")[-1] + "__" + \
                     str(split_num_deal) + '_encode_' + '.jpg'

            cv2.imwrite(path_s, s_1)
            files = {'smfile': open(path_s, 'rb')}
            res_s = requests.post(url, files=files, headers=headers).json()  # 返回json
            res_s_url = res_s['data']['url']  # 返回的url
            res_s__name = res_s_url[31:46]  # 返回的url对应的文件名
            # print('源视频截图 '+path_s+':'+res_s_url)

            pic_num.append('[url=https://sm.ms/image/' + res_s__name + '][img]' + res_s_url + '[/img][/url] ')

            cv2.imwrite(path_e, e_1)
            files = {'smfile': open(path_e, 'rb')}
            res_e = requests.post(url, files=files, headers=headers).json()
            res_e_url = res_e['data']['url']
            res_e__name = res_e_url[31:46]
            # print('Encode视频截图 ' + path_s + ':' + res_e_url)

            pic_num.append('[url=https://sm.ms/image/' + res_e__name + '][img]' + res_e_url + '[/img][/url]\n')

            split_num_deal -= random_frame
            split_num_deal += split_num
            # print(pic_num)
        return pic_num


if __name__ == "__main__":
    worker01 = PtGenPlus()
    worker01.smms_pic_url = "https://sm.ms/api/v2/upload"  # smms图床上传网址
    worker01.smms_pic_api = "NIxPf1145141919810ATYCTp"  # smms图床API
    worker01.pt_gen_url = "https://ptgen.srvfi.top/"  # ptgen的域名
    worker01.pt_gen_api = "&apikey=8hFJLiSqGRkHcPAu"  # ptgen的APIkey
    worker01.uploader_name = "Keprice@SRVFI-Raws"  # 上传者
    worker01.encode_or_dl = "Encode"  # Encode or WEB-DL or Remux
    worker01.bgm_douban_imdb_url = input("请输入豆瓣，bangumi，IMDB资源详情界面的url：")
    worker01.encode_path = input("请输入Encode资源的路径：")
    worker01.source_path = input("请输入Source资源的路径：")
    worker01.final_info_generate()

```
