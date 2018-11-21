---
title: 安全管理平台长沙安装
date: 2018/11/20 10:11:00
tags: 
- Work
categories: 
- Work
- nsfocus
---

#添加用户

	groupadd mysql
	useradd -g mysql -d /nonexistent -s /bin/false mysql
	groupadd openldap
	useradd -g openldap -d /nonexistent -s /bin/false openldap


# ansible安装

[配置文件](https://ansible-tran.readthedocs.io/en/latest/docs/intro_configuration.html)

	yum install ansible
	cp -R /etc/ansible galaxy
	# vi galaxy/ansible.cfg
	inventory      = ./hosts
	remote_user = deployer

	mkdir roles
	cd roles
	ansible-galaxy init nginx

	ansible-playbook playbook.yml 
	ansible -m shell -a 'hostname' all
	ansible -i ./hosts all -u  deployer -m shell -a 'hostname'
	-b -K


# 依赖包安装

	ssh-cp
	ansible -i ./hosts all -u  deployer -m shell -a 'hostname'
## keepalive

	sudo yum install keepalived

## supervisor

	sudo yum install supervisor
	yum -y localinstall supervisor/*rpm
	
	# vim /usr/lib/systemd/system/supervisord.service
	# supervisord service for systemd (CentOS 7.0+)
	# by ET-CS (https://github.com/ET-CS)
	[Unit]
	Description=Supervisor daemon

	[Service]
	Type=forking
	ExecStart=/usr/bin/supervisord -c /etc/supervisord.conf
	ExecStop=/usr/bin/supervisorctl $OPTIONS shutdown
	ExecReload=/usr/bin/supervisorctl $OPTIONS reload
	KillMode=process
	Restart=on-failure
	RestartSec=42s

	[Install]
	WantedBy=multi-user.target	
	

	systemctl status supervisord