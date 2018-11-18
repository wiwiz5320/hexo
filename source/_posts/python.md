---
title: Python
date: 2018/10/17 20:46:25
tags: Dev
categories: Python开发
---

## 虚拟环境配置
### 1.安装pip
To install pip, securely download get-pip.py. [1]:

	curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py

<!--more-->

Inspect get-pip.py for any malevolence. Then run the following:

	python get-pip.py
Upgrading pip
On Linux or macOS:

	pip install -U pip
On Windows [4]:

	python -m pip install -U pip

国内镜像

	vi ~/.pip/pip.conf
	阿里云 http://mirrors.aliyun.com/pypi/simple/
	中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/ 
	豆瓣(douban) http://pypi.douban.com/simple/ 
	清华大学 https://pypi.tuna.tsinghua.edu.cn/simple/

### 2.安装 virtualenv

	pip install virtualenv

### 3.安装 virtualenvwrapper

	pip install virtualenvwrapper
	#命令生效
	if [ -f /usr/bin/virtualenvwrapper.sh ]; then
	    export WORKON_HOME=$HOME/.virtualenvs
   		source /usr/bin/virtualenvwrapper.sh
	fi

常用命令

	mkvirtualenv dev
	lsvirtualenv -b # 列出虚拟环境
	workon [虚拟环境名称] # 切换虚拟环境
	lssitepackages # 查看环境里安装了哪些包
	cdvirtualenv [子目录名] # 进入当前环境的目录
	cpvirtualenv [source] [dest] # 复制虚拟环境
	deactivate # 退出虚拟环境
	rmvirtualenv [虚拟环境名称] # 删除虚拟环境

### 4.安装pylint

	pip install pylint
[官方说明](http://pylint.pycqa.org/en/latest/ "官方说明")
