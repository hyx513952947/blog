title: vpn
author: 大帅
date: 2018-05-25 15:32:50
tags:
---
[搬瓦工](https://bandwagonhost.com/)
账号：513952947@qq.com 
密码；** w
[服务器地址](https://kiwivm.64clouds.com/main.php)

- 服务器安装：
			
      wget --no-check-certificate -O shadowsocks-all.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-all.sh
    
- 安装wget
	 	
      yum -y install wget 
      
- 更改运行脚本权限
	
      chmod +x shadowsocks-all.sh
      
- 运行脚本
	 
	  ./shadowsocks-all.sh 2>&1 | tee shadowsocks-all.log
      
      
- 保存信息
	
      保存服务ip端口号密码等信息
      
- 终端软件安装
		
      ios：https://itunes.apple.com/us/app/wingy-proxy-for-http-s-socks5/id1178584911?mt=8
      Android：https://github.com/shadowsocks/shadowsocks-android/releases
      windows：https://github.com/shadowsocks/shadowsocks-windows/releases
      ubuntu：sudo apt-get update
      		sudo apt-get install python-pip
			  sudo apt-get install python-setuptools m2crypto
              pip install shadowsocks
              sudo apt install shadowsocks
              sslocal -s [ip地址] -p [端口号] -k [密码] -l [1080本地端口] -t [超时] -m aes-256-cfb