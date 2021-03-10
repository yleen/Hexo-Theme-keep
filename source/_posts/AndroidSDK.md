---
title: SDK接入问题 #文章页面上的显示名称，可以任意修改，不会出现在URL中
# date: 2020-12-20 15:30:16 #文章生成时间，一般不改，当然也可以任意修改
categories: #文章分类目录，可以为空，注意:后面有个空格
tags: [SDK,Android] #文章标签，可空，多标签请用格式[tag1,tag2,tag3]，注意:后面有个空格
description: AndroidSdk接入的坑
---



## 1.1   Android SDK 问题

### 1.1.1   Android SDK 及 NDK安装及路径问题

报错如下

![img](C:/Users/Lei Yu/OneDrive - bupt.edu.cn/Document/MarkDownImg/clip_image002.jpg)

解决方法

AndroidSDK与NDK路径需尽量短，否则会报无法识别路径错误，这是个大坑。

### 1.1.2   AndroidSDK与发布版本匹配问题

报错如下

![img](C:/Users/Lei Yu/OneDrive - bupt.edu.cn/Document/MarkDownImg/clip_image004.jpg)

解决方法

首先需要将SDK与预发布版本相对应，一般情况下应该安装不同版本的SDK。之后需要在SDK路径下的tools/bin 执行命令./sdkmanager.bat –licenses