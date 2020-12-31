---
title: Win和Mac使(偷)用(懒)指(技)南(巧) #文章页面上的显示名称，可以任意修改，不会出现在URL中
# date: 2020-12-20 15:30:16 #文章生成时间，一般不改，当然也可以任意修改
categories: #文章分类目录，可以为空，注意:后面有个空格
tags: [Win,Mac,系统] #文章标签，可空，多标签请用格式[tag1,tag2,tag3]，注意:后面有个空格
description: win10 和 Macos快捷方式
---



# Win

## 如何快速格式化硬盘 `Win`

1. 打开cmd
2. 输入diskpart
3.  <img src="https://i.loli.net/2020/12/31/DLGR69srZchzKTI.png" alt="image.png" style="zoom: 50%;" />

## PowerToys使用指南
- 1. spotlight：```Alt+space```
- 2. Windows 快捷键说明：```长按Windows键3秒```


# Mac



## 显示隐藏文件
### 快捷键

cmd+shift+.

### 命令行

//显示隐藏文件
defaults write com.apple.finder AppleShowAllFiles -bool true
//不显示隐藏文件
defaults write com.apple.finder AppleShowAllFiles -bool false

需要重启Finder：窗口左上角的苹果标志-->强制退出-->Finder-->重新启动

## 显示文件路径
使用终端命令行显示完整路径
```
defaults write com.apple.finder _FXShowPosixPathInTitle -bool TRUE;killall Finder
```
隐藏完整路径
```
defaults delete com.apple.finder _FXShowPosixPathInTitle;killall Finder
```

## 程序坞放大效果

程序坞设置 勾选放大

## 键盘映射

键盘->修饰键  option和command互换

## 触发角

系统偏好设定——> Mission Control——> 触发脚...——> 在右下角选择“桌面” ——> 搞定。

## 自动挂载EFI文件

使用命令`diskutil list`找到EFI所在的分区

![image.png](https://i.loli.net/2020/11/24/FlZaNjwgGMW3Oms.png)

获取EFI分区UUID`sudo diskutil info disk0s1 | grep 'Partition UUID'`

![diskutil info](https://i.loli.net/2020/11/24/xLbvwNAD9hMd5T1.png)

F9399EFF-E7E7-42CB-A352-A285E6FC61FA

打开系统自带的自动操作程序，依次点击应用程序 -> 选取 -> 运行 shell 脚本
```
#!/bin/bash

mountEFI=$(echo '你的密码' | sudo -S diskutil info 你的UUID | grep 'Device Node')
echo '你的密码' | sudo -S diskutil mount '/'${mountEFI#*/}
open /Volumes/EFI/EFI/OC
```
将以上代码写入，保存即可
![image.png](https://i.loli.net/2020/11/24/UTRn57Et3i4brLp.png)

参考：https://www.bugprogrammer.me/2019/12/03/mountEFI.html