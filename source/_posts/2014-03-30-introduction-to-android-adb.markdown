---
layout: post
title: "introduction to android adb"
date: 2014-03-30 12:23
comments: true
categories: 
---

Recently, I'm interesting how android adb works. If you are an Android programer, you certainly familiar with `adb shell`,`adb logcat`. Or maybe you only use eclipse, it is helpful to know behind DDMS there is adb.

#ADB overview
We can find adb description from android developer website.
>Android Debug Bridge (adb) is a versatile command line tool that lets you communicate with an emulator instance or connected Android-powered device. It is a client-server program that includes three components:

As introducted by google, it has three key components.

>* A client, which runs on your development machine. You can invoke a client from a shell by issuing an adb command. Other Android tools such as the ADT plugin and DDMS also create adb clients.
>* A server, which runs as a background process on your development machine. The server manages communication between the client and the adb daemon running on an emulator or device.
>* A daemon, which runs as a background process on each emulator or device instance.

#Basic communication model
We can make it clear through this picture.
![adb overview](/images/2014-03-25_adb_overview.png)

* Daemon: which named adbd is a service which is started when android init, and it will be restarted when it died for some reason.

* Service: run background on host, it act as a proxy use for transport. Or you may know android use binder to communication.
* Client: like `adb shell`, `adb logcat`


#Command Detail
Please reference [android developer](http://developer.android.com/tools/help/adb.html)

# How can I connect to Android with ADB over TCP?
At last, we have this topic. As we know from above, adb use tcp/usb, so it certainly can be connected with network. If it works, we can debug android without usb, and more we can connect one device with mutil adb client by many developers. And I find solution like this:
###if your device is rooted
According to a post on stackoverflow, you can enable ADB over Wi-Fi from the device with the commands:

---
`su`

`setprop service.adb.tcp.port 5555`

`stop adbd`

`start adbd`

---
And you can disable it and return ADB to listening on USB with

`setprop service.adb.tcp.port -1` 

`stop adbd`

`start adbd`

###if you have USB access already
It is even easier to switch to using Wi-Fi, if you already have USB. From a command line on the computer that has the device connected via USB, issue the commands

`adb tcpip 5555`

`adb connect IP:5555`

###return to listening over USB
Apps to automate the process by:

`adb usb` 

###proctocol
* server
	* success: OKAY
	* fail: FAIL
* client
	* Message component with head which is 4 bytes indicate length of message, and follow is content. 

###other
There are also several apps on Google Play that automate this process. 

#Reference
[lf_abs12_kobayashi](http://events.linuxfoundation.org/images/stories/pdf/lf_abs12_kobayashi.pdf)

[android developer](http://developer.android.com/tools/help/adb.html)