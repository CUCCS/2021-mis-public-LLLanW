# OpenWrt虚拟机搭建

## 实验目的

* 熟悉基于OpenWrt的无线接入点（AP）配置  
* 为第二章，第三章和第四章实验准备好“无线软AP”环境  

## 实验环境

* 主机`Windows 10`，openwrt-`Linux`  
* VirtualBox  
* 无线网卡`NetGear, Inc. WNA1100 Wireless-N 150 [Atheros AR9271]`  

## 实验要求

* 对照第一章实验无线路由器/无线接入点（AP）配置列的功能清单，找到在OpenWrt中的配置界面并截图证明；  
* 记录环境建设步骤；  
* 如果USB无线网卡能在OpenWrt中正常工作，则截图证明；  
* 如果USB无线网卡不能在OpenWrt中正常工作，截图并分析可能的故障原因并导致可能的解决方法。  

## 配置界面截图

* 重置和恢复AP到出厂默认设置状态  

![16](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp01/img/16.png)  

* 设置AP的管理员用户名和密码  

![26](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp01/img/26.png)  

* 设置SSID广播和非广播模式  

![18](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp01/img/18.png)  

* 配置不同的加密方式  

![19](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp01/img/19.png)  

* 设置AP管理密码  

![17](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp01/img/17.png)  

* 配置无线路由器使用自定义的DNS解析服务器  

![25](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp01/img/25.png)  

* 配置DHCP和禁用DHCP  

![23](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp01/img/23.png)  

* 开启路由器/AP的日志记录功能（对指定事件记录）  

![24](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp01/img/24.png)  

* 配置AP隔离(WLAN划分)功能  

![21](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp01/img/21.png)  

* 设置MAC地址过滤规则（ACL地址过滤器）

![20](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp01/img/20.png)  

* 查看WPS功能的支持情况（未找到）
* 查看AP/无线路由器支持哪些工作模式  

![22](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp01/img/22.png)  

## 实验步骤

### 安装Openwrt

* 根据[第一章实验课件](https://c4pr1c3.github.io/cuc-mis/chap0x01/exp.html)下载安装  

![1](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp01/img/1.png)  
![2](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp01/img/2.png)  
![3](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp01/img/3.png)  
![4](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp01/img/4.png)  

* 在生成vdi文件时，出现`VBoxManage：command not foundCommand not found`的现象，按照[本文档](https://jingyan.baidu.com/article/597a064387244a702b5243e0.html)操作即可解决  

* 在生成vdi时，**一定记得进行扩容操作**，否则后续由于空间不足无法进行实验

### 配置Openwrt网络

* 初次进入  

![5](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp01/img/5.png)  

* `vim etc/config/network`修改HostOnly网卡IP地址  

![6](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp01/img/6.png)  

* 重启该网卡  

![7](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp01/img/7.png)  

* `ip a`验证是否配置成功  

![8](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp01/img/8.png)  

* 主机输入对应网址，成功加载openwrt界面  

![9](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp01/img/9.png)  

### 开启AP功能

* 根据[课件](https://c4pr1c3.github.io/cuc-mis/chap0x01/exp.html)配置`luci`及`usbutils`,安装后**及时重启**

* 查看openwrt是否已经安装网卡驱动

![10](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp01/img/10.png)  

* 查看本网卡的芯片信号，为`AR9271`，故执行`opkg find kmod-* | grep 9271`安装驱动  

![11](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp01/img/11.png)  

* `opkg install hostapd wpa-supplicant`使openwrt支持无线安全机制

* 重启openwrt，刷新网页，发现工具栏中新增`Wireless`选项，则网卡配置成功

![12](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp01/img/12.png)  

* 进入`wireless`栏，进行如下配置

![13](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp01/img/13.png)  

* 启动该网卡  

![14](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp01/img/14.png)  

* 用手机搜索并连接openwrt  

![15](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp01/img/15.png)  
