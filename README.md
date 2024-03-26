# [开源]io.net worker状态监控工具

作者:2xqian

## 前言

* 由易语言编写，考虑到安全问题，只放出源码，不提供编译好的软件。
* 这更倾向于为有编程基础的人提供思路与经验。
* 请勿打开陌生人发送的.exe文件。

## 介绍

这是我为自己和朋友制作的软件，它的数据是从官方API获取的。

* 将worker的运行状态、网速、正常运行时长显示在同一界面
* 可监控不同账号的worker，只需要导入worker的设备id即可
* 每隔10分钟查询一次，双击可以立即查询，单击可以复制docker启动命令(方便在worker掉线时快速复制到对应命令进行重启)
* 可以开启HTTP API，使得worker所在的设备可以查询到自己的状态，从而实现掉线自动重连

![](1.jpg)

## 思路

这种需要操作界面的小工具我会使用易语言来写，耗时不到两小时就可以写出来。

官方的API很好抓，在worker页面按下f12打开控制台就可以看到，每次查询都需要刷新一次access_token，而access_token则需要用api_key和refresh_token获取，其中refresh_token只能使用一次，每次刷新后需要记录返回的新refresh_token用于下一次刷新。

这也意味着你用于这个软件的账号不能再去打开io.net，因为一旦从浏览器上打开io.net，refresh_token就会在浏览器那边被用掉，这边监控就断掉了。

现在是2024年3月25日17:37:31，io.net的ui bug还是很多，有些信息是不准的。

中国都是开vpn连的，感觉都挺不稳定的，所以会添加http api功能，这样就可以在设备上写一个脚本，定时通过这个查询端查worker状态，如果不是up那就自动运行docker的重启指令来实现自动重连。
