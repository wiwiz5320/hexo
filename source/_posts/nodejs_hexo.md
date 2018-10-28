---
title: Hexo建站
date: 2018/10/23 22:53:00
tags: Tool
---

# hexo建站
[参考](https://hexo.io/zh-cn/docs)
## 准备
1.安装git  [win](https://git-scm.com/download/win)

<!--more-->

win下设置git bash代理方法

	export http_proxy="http://127.0.0.1:1083" 
	export https_proxy="http://127.0.0.1:1083" 
	# 如果报错SSL certificate problem: unable to get local issuer certificate
	git config --global http.sslVerify false 

2.安装[npm](https://nodejs.org/dist/v8.12.0/node-v8.12.0-linux-x64.tar.xz)

	安装包安装
	# tar xf  node-v10.9.0-linux-x64.tar.xz       // 解压
	# cd node-v10.9.0-linux-x64/                  // 进入解压目录
	# ./bin/node -v                               // 执行node命令 查看版本
	ln -s /opt/node-v8.12.0-linux-x64/bin/npm    /usr/local/bin/ 
	ln -s /opt/node-v8.12.0-linux-x64/bin/node   /usr/local/bin/

[编译安装](http://www.runoob.com/nodejs/nodejs-install-setup.html)

通过[nvm安装](https://hexo.io/zh-cn/docs/index.html)

	$ curl https://raw.github.com/creationix/nvm/v0.33.11/install.sh | sh
	nvm install stable


**修改源地址为淘宝 NPM 镜像**

	npm config set registry http://registry.npm.taobao.org/
**修改源地址为官方源**

	npm config set registry https://registry.npmjs.org/


## 安装Hexo

	npm install -g hexo-cli

## 建站
安装 Hexo 完成后，请执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件

	$ hexo init <folder>
	$ cd <folder>
	$ npm install
新建完成后，指定文件夹的目录如下：

	./doc/
	├── _config.yml
	├── node_modules
	├── package.json
	├── package-lock.json
	├── scaffolds
	├── source
	└── themes

启动服务器

	hexo server
这时候，访问 http://localhost:4000/ 就可以看到


## 主题
Hexo提供了默认主题landscape，[其他主题](https://hexo.io/themes/)。如果使用[**Next主题**](https://theme-next.iissnan.com/getting-started.html)，方法：

	$ cd blog
	$ git clone https://github.com/theme-next/hexo-theme-next themes/next/
然后在修改/blog/config.yml文件，将其中的theme改成next

	theme: next	
### 添加按钮
Next主题默认按钮是Home、Archive，我们一般会加上tag about category serach。主题的文件夹(./themes/next/)下找到_config.yml文件。在主题配置文件找到menu字段:

	menu:
	  home: / || home
	  #about: /about/ || user
	  tags: /tags/ || tags
	  #categories: /categories/ || th
	  archives: /archives/ || archive
	  #schedule: /schedule/ || calendar
	  #sitemap: /sitemap.xml || sitemap
 	 #commonweal: /404/ || heartbeat


https://www.jianshu.com/p/84a8384be1ae
https://callmewing.com/2017/04/17/Hexo%E5%8D%9A%E5%AE%A2%E9%9B%86%E6%88%90GitBook/

https://juejin.im/post/5a6ee00ef265da3e4b770ac1
https://theme-next.iissnan.com/third-party-services.html#algolia-search

