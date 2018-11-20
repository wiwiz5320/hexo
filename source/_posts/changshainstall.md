---
title: 安全管理平台长沙安装
date: 2018/11/20 10:11:00
tags: 
- Work
categories: 
- Work
- nsfocus
---

# ansible安装

[配置文件](https://ansible-tran.readthedocs.io/en/latest/docs/intro_configuration.html)

	cp -R /etc/ansible galaxy
	# vi galaxy/ansible.cfg
	inventory      = ./hosts
	remote_user = deployer

	ansible-playbook playbook.yml 
	ansible -m shell -a 'hostname' all
	ansible -i ./hosts all -u  deployer -m shell -a 'hostname'

-b -K

# 依赖包安装

	ssh-cp
	ansible -i ./hosts all -u  deployer -m shell -a 'hostname'
## keepalive

	sudo yum install keepalived