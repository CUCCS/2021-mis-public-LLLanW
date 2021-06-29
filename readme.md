# Android 缺陷应用漏洞攻击实验

## 实验目的

* 理解 Android 经典的组件安全和数据安全相关代码缺陷原理和漏洞利用方法；  
* 掌握 Android 模拟器运行环境搭建和 ADB 使用；  

## 实验环境

* [Android-InsecureBankv2](https://github.com/c4pr1c3/Android-InsecureBankv2)  
* Android Studio(在第五章实验时已安装完)
* WIndows10

## 实验要求

* 详细记录实验环境搭建过程；  
* 至少完成以下实验 ：  
  * [x] Developer Backdoor  
  * [x] Insecure Logging  
  * [x] Android Application patching + Weak Auth  
  * [x] Exploiting Android Broadcast Receivers  
  * [x] Exploiting Android Content Provider  
* （可选）使用不同于 Walkthroughs 中提供的工具或方法达到相同的漏洞利用攻击效果；  
  * 推荐 drozer  

## 实验过程

### 环境搭建

#### 配置python2.7

* 下载[python2.7.18安装包](https://www.python.org/ftp/python/2.7.18/python-2.7.18.msi)并按提示顺序安装  

* 修改环境变量，在`path`中加入以下内容。
  * 注1：若电脑同时存在python2和python3，需要把新增内容置前  
  * 注2：安装完成后，找到python3的安装位置，将该文件夹中`python.exe`和`pythonw.exe`重命名为`python3.exe`和`pythonw3.exe`,**否则python2无法成功运行**

![py](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp08/img/8installpy2-2.png)  

* 在命令行中验证是否安装成功

![py](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp08/img/8installpy2-3.png)  

#### 下载仓库及安装相关功能

* `git clone https://github.com/c4pr1c3/Android-InsecureBankv2.git`；`pip install install -r requirements.txt`  

![install](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp08/img/8installre.png)  

* 运行服务器：`python app.py`  
  * 注：运行中会出现若干找不到库的报错问题，按照提示逐个安装即可  

![app](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp08/img/8successapp.png)  

#### 配置adb

* 按照[本文的方法](https://blog.csdn.net/weixin_38818810/article/details/81014945)安装并配置`adb`  

![adb](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp08/img/8installadb.png)  

#### 安装`InsecureBankv2.apk`

* 如图，**在AVD打开的情况下**安装apk文件  

![apk](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp08/img/8installapk.png)  

* 在AVD中找到`InsecureBankv2`程序，验证登录：帐号`jack`密码`Jack@123$`或帐号`dinesh`密码`Dinesh@123$`  

![apk](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp08/img/8success.png)  

* 测试是否登录成功  

![apk](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp08/img/8login.png)  

### Developer Backdoor

* 安装`JADX`及[dex2jar](https://sourceforge.net/projects/dex2jar/files/dex2jar-2.0.zip/download)  

  ```1
  #JADX
  git clone https://github.com/skylot/jadx.git
  cd jadx
  gradlew.bat dist
  ```

* 解压缩`InsecureBankv2.apk`,将`classes.dex`复制到`dex2jar`的目录中

* 转换格式：`bat`转换为`jar`  

![exp01](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp08/img/8change.png)  

* 在jadx文件夹的`\build`中找到`jadx-GUI`，用该程序打开`classes-dex2jar.jar`  

* 如图，该漏洞导致当用户名为`devadmin`时，使用**任意值(包括缺省)**均可登录该程序  
![exp01](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp08/img/8exp10.png)  

![exp01](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp08/img/8exp1.png)  

![exp01](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp08/img/8exp11.png)  

### Insecure Logging

* 在命令行打开日志`apk logcat`  
* 登录账户，并修改密码  

![exp02](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp08/img/8changepw.png)  

* 查看日志，发现登录及修改密码的信息均以明文存储  

![exp02](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp08/img/8log1.png)  
![exp02](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp08/img/8log2.png)  

### Android Application patching + Weak Auth

* **法一**：利用`APKtool`，但因为后续需要单独安装签名功能，故选择法二完成该实验
  * 按[本文方法](https://blog.csdn.net/u010659278/article/details/45568385/)配置`APKtool`
  * 反编译：`apktool d InsecureBankv2.apk`
  * 在反编译后的文件夹中找到`\res\value\strings.xml`
  ![exp031](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp08/img/8findxml.png)  
  * 打开，找到图中位置，将`no`修改成`yes`  
  ![exp031](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp08/img/8noadmin.png)  
  ![exp031](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp08/img/8mode.png)  
  * 重打包：`apktool b InsecureBankv2`
  * **将AVD中原程序删除**，安装新生成的apk，发现由于没有签名无法安装，进行法二。

* **法二**：利用`VScode`中的`APKLab`扩展。此方法操作更加简便。可根据[本文的方法](https://my.oschina.net/tavenli/blog/4889086)直接生成
  * 在`VScode`中使用`CTRL+SHIFT+P`打开`APKLab`  
  * 打开`APK`文件，找到`\res\value\strings.xml`，按`法一`修改  
  * 右键点击列表最下方`apktool.yml`重打包生成新文件  
  ![exp032](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp08/img/8apklab.png)  
  * 重新安装，发现界面中新增`创建账户`的按钮  
  ![apk](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp08/img/8installapk.png)  
  ![exp032](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp08/img/8exp3.png)  

### Exploiting Android Broadcast Receivers

* 反编译：`apktool d InsecureBankv2.apk`  
* 在文件中找到`AndroidManifest.xml`文件并打开  
![exp4](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp08/img/8afterenc.png)  
* 在`AndroidManifest.xml`中找到该行
![exp4](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp08/img/8afterenc1.png)  
* 在jadx文件夹的`\build`中找到`jadx-GUI`，用该程序打开`classes-dex2jar.jar`，在`MyBroadCastReceiver`中找到该行  
![exp4](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp08/img/8param.png)  
* 通过`adb shell`进行密码修改  

```2
adb shell
am broadcast -a theBroadcast -n com.android.insecurebankv2/com.android.insecurebankv2.MyBroadCastReceiver --es phonenumber 5554 --es newpass Dinesh@123!
```

![exp4](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp08/img/8changepwnew.png)  

* 打开`AVD`,发现收到新信息  
![exp4](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp08/img/8receivemess.png)  

### Exploiting Android Content Provider

* 反编译：`apktool d InsecureBankv2.apk`  
* 在文件中找到`AndroidManifest.xml`文件并打开，找到该行  
![exp5](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp08/img/8trackuser.png)  
* 在jadx文件夹的`\build`中找到`jadx-GUI`，用该程序打开`classes-dex2jar.jar`，在`TrackUserContentProvider`中找到该行  
![exp5](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp08/img/8trackuser1.png)  
* 通过`adb shell`查看用户登录信息  

```3
adb shell
content query --uri content://com.android.insecurebankv2.TrackUserContentProvider/trackerusers
```

![exp5](https://github.com/CUCCS/2021-mis-public-LLLanW/blob/exp08/img/8exp05.png)  

## 遇到的问题及解决

* 安装`python2`后无法运行
  * 解决：安装完成后，找到python3的安装位置，将该文件夹中`python.exe`和`pythonw.exe`重命名为`python3.exe`和`pythonw3.exe`  
* 新生成的apk无法安装
  * 解决：根据报错查阅发现是因为模拟器已存在该程序，二者签名不一致无法安装。故把模拟器中原有的程序删除即可

## 参考资料

* [Windows10安装adb](https://blog.csdn.net/weixin_38818810/article/details/81014945)
* [windows下apktool的安装和使用方法](https://blog.csdn.net/u010659278/article/details/45568385/)
* [WIndows下dex2jar下载链接](https://sourceforge.net/projects/dex2jar/)
* [VScode中APKLab的使用](https://my.oschina.net/tavenli/blog/4889086)
