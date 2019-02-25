---
title: 安全管理平台长沙安装
date: 2018/11/20 10:11:00
tags: 
- Work
categories: 
- Work
- nsfocus
---

# KVM快照

	#查看快照
	virsh snapshot-list centos1
	#创建快照
	virsh snapshot-create-as centos1 centos1_sn1
	#恢复快照
	virsh snapshot-revert --domain centos1 centos1_sn1
	


# 配置免密

	ssh-keygen
	mkdir ~/.ssh
 	mv id_rsa.pub authorized_keys
	chmod 700 /home/your_user/.ssh
	chmod 600 authorized_keys

# centos7 配置console

	systemctl enable serial-getty@ttyS0.service
	systemctl start serial-getty@ttyS0.service

# 配置sudo权限

	chmod +w /etc/sudoers
	# vi /etc/sudoers
	deployer        ALL=(ALL)       NOPASSWD: ALL
	chmod -w /etc/sudoers
	

#添加用户

	groupadd mysql
	useradd -g mysql -d /nonexistent -s /bin/false mysql
	groupadd openldap
	useradd -g openldap -d /nonexistent -s /bin/false openldap


#firewall命令

	firewall-cmd --zone=public --list-ports
	firewall-cmd --zone=public --add-port=3306/tcp --permanent
	firewall-cmd --zone=public --remove-port=8081/tcp --permanent
	firewall-cmd --reload

# ansible安装

[安装说明](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)

[配置文件](https://ansible-tran.readthedocs.io/en/latest/docs/intro_configuration.html)

	pip install ansible
	# yum install ansible
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

	# 配置防火墙策略时注意需要指定网卡
	sudo yum install keepalived
	firewall-cmd --direct --permanent --add-rule ipv4 filter INPUT 0   --in-interface eth0 --destination 224.0.0.18 --protocol vrrp -j ACCEPT
	firewall-cmd --direct --permanent --add-rule ipv4 filter OUTPUT 0  --out-interface eth0 --destination 224.0.0.18 --protocol vrrp -j ACCEPT
	firewall-cmd --reload


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