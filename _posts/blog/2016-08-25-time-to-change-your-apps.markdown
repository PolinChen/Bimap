---
layout: post
title:  "Microsoft IIS log 設定方式"
date: 2016-08-25 16:54:46
author: Admin
categories: 
- ELK 
- IIS
img: http://i.imgur.com/bGySN8e.png
thumb: fa-cog
---

# iis 6.0 上的參數設定
用戶提供的iislog 中的參數中， 沒有time taken 的內容

```
Software: Microsoft Internet Information Services 6.0
Version: 1.0
Date: 2016-06-28 06:05:45
Fields: date time s-sitename s-ip cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) sc-status sc-substatus sc-win32-status
```

## 回答內容

> 目前的iis log 中 有個Time Taken的參數， 是看響應時間， default 是N， 所以沒有收集到， 是否可以更改為Y， 以下是microsoft 的iis log 的說明文件
> Time Taken time-taken The length of time that the action took, in milliseconds.  N

> 目前的iis log 中 有個Time Taken的參數， 是看響應時間， default 是N， 所以沒有收集到， 是否可以更改為Y， 以下是microsoft 的iis log 的說明文件


## microsoft 上的設定畫面

![設定的內容](http://i.imgur.com/bGySN8e.png) 

## iislog 時間差的確認

![時間差的畫面](/img/iislog-8hr.png)


microsoft 上的說明內容



[microsoft iis 6.0 說明文件](https://www.microsoft.com/technet/prodtechnol/WindowsServer2003/Library/IIS/676400bc-8969-4aa7-851a-9319490a9bbb.mspx?mfr=true)

