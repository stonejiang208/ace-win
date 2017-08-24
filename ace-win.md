# 搭建ACE及TAO开发环境的快速方法 (Windows篇)

Stone

## 摘要

本文介绍如何在Windows下快速搭建ACE及TAO的开发环境。关键步骤为：
 1. 获得ACE及TAO源代码
 2. 准备预备环境
 3. 编译ACE及TAO库
 4. 验证及使用库
 5. 简单介绍多平台构建工具mwc的用法


## 简介

本文暂不过多介绍细节，只上干货。欲知详情，请见官网：

 - ACE 的官网：http://www.cs.wustl.edu/~schmidt/ACE.html

 - TAO 的官网：http://www.cs.wustl.edu/~schmidt/TAO.html


## 演示环境

    * Windows 7
    * Visual Studio 2015 社区版
    * ACE 6.4.4
    * mwc 4.1.25 (in ACE )
    * Active Perl 5.22.1
    * git 2.9.0 windows.1


## 获取ACE及TAO源代码

通常可以从官网或github获取源码。二者略有差异。

官网下载站点为：http://download.dre.vanderbilt.edu

官网把源代码分为：
  - ACE+TAO (内含MPC)
  - CIAO (本文不讨论)
  - DAnCE (本文不讨论)
  
按版本，分为
  - Latest Micro Release (具有最近小更新的版本)， 当前为 x.4.4,其中 ACE为6.4.4，TAO为2.4.4
  - Latest Minor Release （最新的次版本更新），当前为x.4.0。
  
按是否含用构建文件(Makefile,sln等工程文件),分为:
  - Full
  - Sources Only （建议用仅源代码，因为工程文件我们可以通过mwc工具重新生成）
  
按下载方式，分为
  - HTTP
  - FTP

按下载方式，分为
  - tar.gz
  - tar.bz2 (更小，Linux用户推荐)
  - zip (Windows环境下推荐)


### 从官方网站获取源码

本文建议下载的链接包为

http://download.dre.vanderbilt.edu/previous_versions/ACE+TAO-src-6.4.4.zip

然后请把源代码解压至 dre/目录下，目录结构如下：

```
C:\DRE
└─ACE_wrappers
    ├─ace
    │  ├─Compression
    │  │  └─rle
    │  ├─ETCL
    │  ├─FlReactor
    │  ├─FoxReactor
    │  ├─Monitor_Control
    │  ├─os_include
    │  │  ├─arpa
    │  │  ├─net
    │  │  ├─netinet
    │  │  └─sys
    │  ├─QoS
    │  ├─QtReactor
    │  ├─SSL
    │  ├─TkReactor
    │  ├─XML_Utils
    │  │  ├─XMLSchema
    │  │  └─XSCRT
    │  └─XtReactor
    ├─ACEXML
    ...
    （下略）
```


### 从github获取源码


如果从github上抓取源代码，需要分别抓两个仓库
 - MPC
 - ACE+TAO
 
```
mkdir dre
cd dre
git clone -b master --depth 1 https://github.com/DOCGroup/ACE_TAO.git
git clone -b master --depth 2 https://github.com/DOCGroup/MPC.git
```

## 准备预备环境
 * Windows 7 (本文采用的环境，Windows其它平台也支持)
 * Visual Studio 2015 （本文采用的是社区版）
 * 安装Active Perl 本文采用的是5.22.1，官网如下:
     https://www.activestate.com/activeperl
 * 安装git客户端
 
## 编译ACE+TAO库

编译步骤可分为4步：

 1. 设置环境变量
 2. 创建config.h
 3. 生成工程文件
    （以上三步不分先后顺序）
 4. 用Visual Studio 打开工程文件,并全编译

### 设置环境变量
  ```
  set ACE_ROOT=c:\dre\ACE_wrappers
  set TAO_ROOT=%ACE_ROOT%\TAO
  set PATH=%PATH%;%ACE_ROOT%\bin;%ACE_ROOT%\lib
  ```
 
 Windows下设置环境变量正确姿势请参考：
 ~~~
 http://jingyan.baidu.com/article/d5a880eb6aca7213f047cc6c.html
 ~~~
 
如果编译时出现什么幺蛾子，多半设置环境变量不正确或未生效，
验证环境变量是否成效，请重新打开dos shell，执行以下指令：
```
echo %ACE_ROOT%
echo %TAO_ROOT%
echo %PATH%
cd %TAO_ROOT%
```

### 创建 config.h
在%ACE_ROOT%\ace目录下，新建config.h
```
cd %ACE_ROOT%\ace
```

config.h文件内容如下
```
#include "ace/config-win32.h"
```

### 重新生成

进入到%TAO_ROOT%目录下，执行以下指令：
```
cd %TAO_ROOT%
mwc.pl -type vc14 --name_modifies "*_vc14" TAO_ACE.wmc
```

大概两分钟左右时间，mwc会为大约220个工程创建vcxproj文件以及一个TAO_ACE.sln文件。
这220个工程中并没有包含源代码中的示例工程和测试工程。

### 编译

用Visual Studio打开上一步生成的TAO_ACE.sln文件，全编译，将在 %ACE_ROOT%\lib目录中生成库文件。
编译时可以选择Debug,或Release方式编译，还可以选择是win32或x64平台。学习目上的建议编译为x64的Debug版，生产环境下，建方编译为x64的Release版。


## 验证是否编译成功

源代码取自%TAO_ROOT%\test\Hello

https://github.com/stonejiang208/ace-win.git



