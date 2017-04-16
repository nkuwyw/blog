---
layout: post
title:  "aircrack-ng渗透笔记"
subtitle: "唉..毕竟是年纪大了"
date:   2017-04-15 22:34:01
categories: [tech]
tag: [markdown]
---
___
#上了岁数记性越来越不好,前几天成功渗透过的步骤过段时间竟然很艰难的才能回忆起其中的片段,看来记下笔记是不可或缺的了.
1.自动检查冲突进程并结束:airmon-ngcheck kill
2.首先修改网卡工作状态:airmon-ng start wlan0
3.运行探测程序侦查周围AP状态:airodump wlan0mon,按键'a'可以切换显示模式.列参数详解:
BSSID表示AP的MAC地址,当其状态为not asscoiated时代表该设备正在尝试与某个AP建立链接,但仍处于未链接的状态.
PWR代表信号强度,其值越大信号越强
RXQ前10s数据包的成功接受率
Beacons每秒中AP发出的时隙个数,为了保障这些时隙能尽可能远的发送,一个AP每秒以最低的速率(1M)发出10个时隙左右
#Data 捕捉到数据包的总量,包括广播包
#/s前10s每秒捕捉到数据包的数量
CH 工作频道
MB 最大支持速率Maximum  speed  supported  by  the AP. If MB = 11, it's 802.11b, if MB = 22 it's
              802.11b+ and higher rates are 802.11g. The dot (after 54 above) indicates  short
              preamble is supported. 'e' indicates that the network has QoS (802.11e) enabled.

ENC 所使用的加密算法OPN开放式网络,WEP?表示没有充足的数据确定加密算法采用WEP或者WPA或者WPA2中的某一个,其余均为显示结果
CIPHERThe cipher detected. One of CCMP, WRAP, TKIP, WEP, WEP40, or WEP104. Not  manda‐
              tory,  but TKIP is typically used with WPA and CCMP is typically used with WPA2.
              WEP40 is displayed when the key index is greater then  0.  The  standard  states
              that the index can be 0-3 for 40bit and should be 0 for 104 bit.

AUTH 认证方式,通常为PSK(Pre-shared key for WPA/WPA2).
ESSID 接入点名称
STATION 已接入AP的终端及尚未接入AP终端的MAC地址,当尚未接入终端时其BSSID列显示为not assciated
Lost 客户端丢失的数据帧量
Packets 客户端发送出的数据包数
Probe 表示有客户端正在尝试连接但是未尚未成功建立连接的网络.
4.当确定需要渗透的网络后可开始抓包(此窗口需保留):airodump-ng --ivs -c 工作频道 -w 目的文件 --bssid 目标AP MAC(也可使用--essid) wlan0mon 
5.使用aireplay进行数据帧注入,产生数据流量加速获取Handshake Data:aireplay-ng -0 0 -a 目的AP -c 伪装地址 wlan0mon
6.观察抓包窗口状态,当出现handshake表示以抓取到握手包,可以进行暴力破解.
7.爆破:
8.最后不要忘了恢复网卡模式,否则无法联网:airmon-ng stop wlan0mon


