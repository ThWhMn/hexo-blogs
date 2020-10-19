---
title: 手把手带你搭建钓鱼Wi-Fi热点[1]
tags: ""
date: 2020-04-08T12:42:33.772Z
---

原创 年华不散场 物联网IOT安全 _1周前_
<!-- more -->

{% asset_img dihrkq0ll60.webp  %}

Hello，小伙伴们晚上好。今天我们来**搭建一个钓鱼Wi-Fi**，这里需要一台双网卡的电脑（其中一个网卡必须是无线网卡）。**系统采用的是ubuntu18**，安装步骤就省略啦  

  

我们首先查看一下网卡信息：

{% asset_img 20hvymc5edt.webp  %}

需要禁用WI-FI并解锁操作，最后再配置IP地址

```
nmcli radio wifi off
```


**需要注意wlan0是网卡的名字**，像这个ubuntu的网卡名为wlx70f11c127eb9

{% asset_img 56f1ksxsyy4.webp  %}

我们这里选择使用**hostapd**这个软件来创建热点，该软件需要一个配置文件，内容如下：

  

```
interface=wlan0
```

{% asset_img 1rtdb9d39zf.webp  %}  

**这里有个坑，等号左右不能有空格**  

  

启动hostapd

```
sudo hostapd ./open.conf
```

{% asset_img 21u0gyanruv.webp  %}

如此一来，我们的Wi-Fi热点就创建成功了。

{% asset_img d9q0w338rqo.webp  %}

但是我们还没有**配置dns和dhcp服务器**

  

使用apt安装isc-dhcp-server：

{% asset_img 2cxouuuq3r2.webp  %}

 **修改/etc/default/isc-dhcp-server配置文件**，将网卡改为我们的无线网卡：

{% asset_img wwoh68xn60.webp  %}

**修改/etc/dhcp/dhcpd.conf配置文件**，编辑DHCP地址池以及DNS服务器地址等等

{% asset_img 8iarq4kjjk8.webp  %}

**启动DHCP服务**  

{% asset_img 772khsiyc6.webp  %}

还有最后一步就是要**将wlan网卡上接收到的流量转发到有线网卡上**，这样我们才可以让钓鱼Wi-Fi内的客户端上网。  

```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```

{% asset_img 9ll88vyrimw.webp  %}

我们来试试能不能上网：

{% asset_img 2htu45n0on2.webp  %}

Nice，现在我们已经完成第一步啦，**在linxu上共享了一个热点**，下一篇文章我们将要介绍抓取图片、HTTP敏感信息等等。

**点我留言**

  

{% asset_img 7i1s2gvsalc.gif  %}

{% asset_img 26yyei0sqsg.png  %}

  

广告时间  

  

{% asset_img 2dqcn25h4py.webp  %}