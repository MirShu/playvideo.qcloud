# 腾讯超级视频播放器加密视频问题

使用超级播放器播放视频，若有如下情况之一：

1、域名开启了 KEY 防盗链。

2、使用了 Default 以外的 超级播放器配置。

3、需要播放 加密 的视频内容。

您需要按照规则生成超级播放器签名。若签名正确，即可播放相应的视频内容。
腾讯开发文档里面指出的如果视频需要加密的步骤

加密视频移动端注意的问题

# 第一点

最近腾讯升级v4版本的地址

```
 private final String BASE_URLS_V2 = "https://playvideo.qcloud.com/getplayinfo/v4";

```

开启防盗链为必填选项
会有些问题，貌似开发文档种没怎么标明，找到腾讯demo 中的

![2781551-6959e432dd466bb8](https://user-images.githubusercontent.com/13359093/211735682-c7f218c5-ff82-4647-ab78-82d2a5acfc28.png)

头部会有个视频前缀地址，这个是重点
地址必须改成 v4版本的

```
https://playvideo.qcloud.com/getplayinfo/v4
```

![2781551-5def13f5657ae1ff](https://user-images.githubusercontent.com/13359093/211735758-de58b071-937c-4761-8aa6-6a205c058e60.png)

# 第二点

同一个工具类 SuperVodInfoLoaderV3中 找到

```
 str.append("sign=" + sign + "&");
```

![2781551-bd9796371ba0c167](https://user-images.githubusercontent.com/13359093/211735949-4deb9310-29a4-4e9f-b92f-5566ef33d260.png)

把sign= 改成 psign=

![2781551-04e49cd501b599f0](https://user-images.githubusercontent.com/13359093/211736028-6a8ecab7-c403-4473-a933-28ffd33f600a.png)

# 最后一点

```
//开启防盗链需填写 psign， psign 即超级播放器签名，签名介绍和生成方式参见链接：https://cloud.tencent.com/document/product/266/42436
SuperPlayerModel model = new SuperPlayerModel();
model.appId = 1400329071;// 配置 AppId
model.videoId = new SuperPlayerVideoId();
model.videoId.fileId = "5285890799710173650"; // 配置 FileId
mSuperPlayerView.playWithModel(model);
model.videoId.pSign = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhcHBJZCI6MTQwMDMyOTA3MSwiZmlsZUlkIjoiNTI4NTg5MDc5OTcxMDE3MzY1MCIsImN1cnJlbnRUaW1lU3RhbXAiOjEsImV4cGlyZVRpbWVTdGFtcCI6MjE0NzQ4MzY0NywidXJsQWNjZXNzSW5mbyI6eyJ0IjoiN2ZmZmZmZmYifSwiZHJtTGljZW5zZUluZm8iOnsiZXhwaXJlVGltZVN0YW1wIjoyMTQ3NDgzNjQ3fX0.yJxpnQ2Evp5KZQFfuBBK05BoPpQAzYAWo6liXws-LzU"; 
mSuperPlayerView.playWithModel(model);
```


```
 superPlayerModel.videoId.fileId = Video_fileid; // 配置FileId
  superPlayerModel.videoId.sign = Video_sign(); //视频签名密钥，（服务端给过来的签名）
```
