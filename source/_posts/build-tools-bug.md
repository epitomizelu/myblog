---
title: build-tools-bug
date: 2017-11-20 16:24:05
tags:
---

# 一，报错信息
Exception in thread "png-cruncher_5" java.lang.RuntimeException: 
Timed out while waiting for slave aapt process, make sure the aapt execute at D:\softpath\androidsdk\build-tools\23.0.3\aapt.exe can run successfully (some anti-virus may block it) 
or try setting environment variable SLAVE_AAPT_TIMEOUT to a value bigger than 5 seconds at com.android.builder.png.AaptProcess.waitForReady(AaptProcess.java:108) at com.android.builder.png.QueuedCruncher$1.creation(QueuedCruncher.java:110) at com.android.builder.tasks.WorkQueue.run(WorkQueue.java:203) at java.lang.Thread.run(Thread.java:722)


# 二，解决方案
1，增加系统环境变量 SLAVE_AAPT_TIMEOUT ，并将其值设置为30。
2，如果还不行，有可能你的build-tool被杀毒软件标记了，从你的同事那里拷贝一份或者从csdn上下载一份替换掉你电脑上的build-tool
![](http://ov4zbm8w2.bkt.clouddn.com/android/bugs/build-tools.png)


 

