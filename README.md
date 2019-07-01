ShadowsocksR
===========

[![Build Status]][Travis CI]

A fast tunnel proxy that helps you bypass firewalls.

个人备份用。

For private backup use.

Server
------

### Install

仅记录个人在CentOS上安装的步骤：

Only recorded my installation steps on CentOS:

    #!/bin/sh
    clear
    echo "              ===简易SSR Manyuser后端程序安装脚本==="
    echo "       ===此脚本将会带您自动安装sspanel前端使用的SSR后端程序==="
    echo "===请确保VPS内存容量大于等于1GB或启用了swap空间，否则可能导致安装失败==="
    echo "            请按任意键继续安装，否则请使用Ctrl+C退出脚本"
    read -n 1
    
    ## Installing dependences 安装依赖
    yum install -y python-setuptools && easy_install pip
    yum install -y git
    yum -y groupinstall "Development Tools"
    
    ## Installing libsodium to enable chacha20 and other advanced encryptions 安装libsodium以启用chacha20等高级加密
    
    wget https://download.libsodium.org/libsodium/releases/LATEST.tar.gz
    
    tar xf LATEST.tar.gz && cd libsodium-stable
    ./configure && make -j2 && make install
    
    ## "make -j2" depends on the cores of your server. If single core, then "make -j1" and vice versa. "make-j2"根据服务器的CPU核心数决定，如果是单核，则"make -j1"，反之亦然
    
    echo /usr/local/lib > /etc/ld.so.conf.d/usr_local_lib.conf
    ldconfig
    
    ## Installing ShadowsocksR 安装SSR
    git clone -b manyuser https://github.com/shadowsocksr-backup/shadowsocksr.git
    cd shadowsocksr
    yum install -y python-devel libffi-devel openssl-devel
    pip install cython
    pip install cymysql
    ## On the server of Vultr and some other VPS provider, swap partition was not being created and can cause cymysql installation fail
    ## on low memory machines (gcc exits status 4). Please create swap partition manually.
    ## 在Vultr及其他部分提供商的主机上，默认没有创建swap分区，这可能导致低内存机器上的cymysql安装失败（gcc exits status 4）
    ## 请手动创建swap分区
    
    ## Creating proper config files. 创建正确的配置文件
    cp apiconfig.py userapiconfig.py
    cp config.json user-config.json
    cp mysql.json usermysql.json
    
    ## Back-end program and database configuration 后端程序与数据库配置
    vim userapiconfig.py
    ## Edit necessary data inside userapiconfig.py then :wq 编辑该文件中的必要数据并保存退出
    vim usermysql.json
    ## Edit necessary data inside usermysql.json then :wq编辑该文件中的必要数据并保存退出
    
    ## If necessary, disable firewall completely to prevent user ports being blocked 如有必要，可以彻底关闭防火墙以免用户端口被屏蔽
    ## For CentOS7:
    systemctl stop firewalld
    systemctl disable firewalld
    ## For CentOS6:
    service iptables stop
    chkconfig iptables off
    
    ## Back-end program installation and configuration complete 后端程序安装与配置完成
    


### Usage for single user on linux platform

If you clone it into "~/shadowsocksr"  
move to "~/shadowsocksr", then run:

    bash initcfg.sh

move to "~/shadowsocksr/shadowsocks", then run:

    python server.py -p 443 -k password -m aes-128-cfb -O auth_aes128_md5 -o tls1.2_ticket_auth_compatible

Check all the options via `-h`.

You can also use a configuration file instead (recommend), move to "~/shadowsocksr" and edit the file "user-config.json", then move to "~/shadowsocksr/shadowsocks" again, just run:

    python server.py

To run in the background:

    ./logrun.sh

To stop:

    ./stop.sh

To monitor the log:

    ./tail.sh


Client
------

* [Windows] / [macOS]
* [Android] / [iOS]
* [OpenWRT]

Use GUI clients on your local PC/phones. Check the README of your client
for more information.

Documentation
-------------

You can find all the documentation in the [Wiki].

License
-------

Copyright 2015 clowwindy

Licensed under the Apache License, Version 2.0 (the "License"); you may
not use this file except in compliance with the License. You may obtain
a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the
License for the specific language governing permissions and limitations
under the License.

Bugs and Issues
----------------

* [Issue Tracker]



[Android]:           https://github.com/shadowsocksr/shadowsocksr-android
[Build Status]:      https://travis-ci.org/shadowsocksr/shadowsocksr.svg?branch=manyuser
[Debian sid]:        https://packages.debian.org/unstable/python/shadowsocks
[iOS]:               https://github.com/shadowsocks/shadowsocks-iOS/wiki/Help
[Issue Tracker]:     https://github.com/shadowsocksr/shadowsocksr/issues?state=open
[OpenWRT]:           https://github.com/shadowsocks/openwrt-shadowsocks
[macOS]:             https://github.com/shadowsocksr/ShadowsocksX-NG
[Travis CI]:         https://travis-ci.org/shadowsocksr/shadowsocksr
[Windows]:           https://github.com/shadowsocksr/shadowsocksr-csharp
[Wiki]:              https://github.com/breakwa11/shadowsocks-rss/wiki
