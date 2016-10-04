---
layout: post
title:  "Window Event Log 的處理方式"
date: 2016-10-03 11:54:46
author: Admin
categories:
- ELK
- Windows
img: https://blackrockbusiness.com/wp-content/uploads/2016/03/error-code-8003.jpg 
thumb: fa-windows
---

# Window Event Log 的處理方式

### [Windows XML Event Log (EVTX)](http://forensicswiki.org/wiki/Windows_XML_Event_Log_(EVTX))

#### 常用Tools
------
- Evtx Parser
- libevtx
- log2timeline
- wevtutil
- LogParser
- python-evtx
- winlast

### [Saving logs from the Event Viewer](http://support.exclaimer.com/topics/638-saving-logs-from-the-event-viewer/)

1. Go to Applications and Services Logs under Event Viewer on the left of the screen.
1. Right click on the Exclaimer file.
1. Click Save All Events As.....
1. The logs will then save as an .evtx file from the Event Viewer.

![evtx](http://support.exclaimer.com/s/attachments/14925/2/1/8562fed8c2d282a62e2779fd5ea66c5c.jpg)

### 檢查 event log 的格式內容在Linux 中

```
DB_EVENT.evtx: MS Windows Vista Event Log, 1 chunks (no. 0 in use), next record no. 2
DB_EVENT.txt:  UTF-8 Unicode (with BOM) English text, with very long lines
```

### [logstash-windows-eventlog](https://github.com/randomvariable/logstash-windows-eventlog/blob/master/README.md)

To collect all registered event logs, use the following config:

```
input {
  windowseventlog {}
}
```

To collect events from just Hyper-V, use a config like :

```
input {
  windowseventlog {
  log_files  => ['Microsoft-Windows-Hyper-V-Hypervisor-Operational','Microsoft-Windows-Hyper-V-Config-Admin']
}
```

Let's capture some IIS tracing logs (from AppFabric or some such):

```
input {
windowseventlog {
  log_files => ['C:\inetpub\logs\tracing\evt.evtx']
  log_type  => 'FilePath'
}
```

Capture all logs, but filter for only a certain system event ID using an xPath query:

```
input {
windowseventlog {
  filter => "*[System[(EventID=2202)]]"
}
```

### log 中文格式範本
```
等級    日期和時間      來源    事件識別碼      工作類別
資訊    2016/10/4 上午 10:48:09 Desktop Window Manager  9003    無      桌面視窗管理員無法啟動，因為目前沒有使用轉譯緩衝處理的佈景主題
資訊    2016/10/4 上午 10:48:08 Microsoft-Windows-Winlogon      4101    無      Windows 授權已驗證。
資訊    2016/10/4 上午 10:37:19 Desktop Window Manager  9009    無      桌面視窗管理員已經結束，錯誤碼是 (0x40010004)
資訊    2016/10/4 上午 09:40:32 SceCli  1704    無      群組原則物件中的安全性原則已經套用成功。
資訊    2016/10/4 上午 09:25:14 Desktop Window Manager  9003    無      桌面視窗管理員無法啟動，因為目前沒有使用轉譯緩衝處理的佈景主題
資訊    2016/10/4 上午 09:25:14 Microsoft-Windows-Winlogon      4101    無      Windows 授權已驗證。
錯誤    2016/10/4 上午 08:30:06 SQLISPackage100 12291   無      "Package ""Load_TWCI_Prestage"" failed."
資訊    2016/10/4 上午 08:30:06 SQLISPackage100 12288   無      "Package ""Load_TWCI_Prestage"" started."
資訊    2016/10/4 上午 08:03:30 SceCli  1704    無      群組原則物件中的安全性原則已經套用成功。
錯誤    2016/10/4 上午 08:00:06 SQLISPackage100 12291   無      "Package ""Load_TWCI_Prestage"" failed."
資訊    2016/10/4 上午 08:00:06 SQLISPackage100 12288   無      "Package ""Load_TWCI_Prestage"" started."
錯誤    2016/10/4 上午 07:30:05 SQLISPackage100 12291   無      "Package ""Load_TWCI_Prestage"" failed."
資訊    2016/10/4 上午 07:30:05 SQLISPackage100 12288   無      "Package ""Load_TWCI_Prestage"" started."
錯誤    2016/10/4 上午 07:00:05 SQLISPackage100 12291   無      "Package ""Load_TWCI_Prestage"" failed."
資訊    2016/10/4 上午 07:00:05 SQLISPackage100 12288   無      "Package ""Load_TWCI_Prestage"" started."
錯誤    2016/10/4 上午 06:30:05 SQLISPackage100 12291   無      "Package ""Load_TWCI_Prestage"" failed."
```
