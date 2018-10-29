---
title: Centos7 KVM环境搭建
date: 2018/10/29 23:11:00
tags: 
- 虚拟化
- Linux
- Centos
categories: 
- Virtualization
- KVM
---

## 常用命令
查看是否支持CPU虚拟化

	grep -E "vmx|svn" /proc/cpuinfo
<!--more-->
查看Centos虚拟化版本

	cat /etc/redhat-release|cat /etc/centos-release
添加VNC端口
	
	firewall-cmd --permanent --zone=public --add-port=5904-5905/tcp
	firewall-cmd --reload

重启网络配置

	systemctl restart NetworkManager
	systemctl restart network

支持的CPU models

	virsh capabilities

## Centos7安装KVM环境
[详细步骤](https://www.cyberciti.biz/faq/how-to-install-kvm-on-centos-7-rhel-7-headless-server/)
### Step 1: Install kvm
Type the following yum command:

	# yum install qemu-kvm libvirt libvirt-python libguestfs-tools virt-install
Start the libvirtd service:

	# systemctl enable libvirtd
	# systemctl start libvirtd

### Step 2: Verify kvm installation
	
	# lsmod | grep -i kvm

### Step 3: Configure bridged networking
Edit `/etc/sysconfig/network-scripts/ifcfg-br0` and add:
	
	BOOTPROTO="dhcp"
	IPV6INIT="yes"
	IPV6_AUTOCONF="yes"
	ONBOOT="yes"
	TYPE="Bridge"
	DELAY="0"
	ONBOOT="yes"
OR

	DEVICE=br1
	TYPE=Bridge  
	BOOTPROTO=none 
	ONBOOT=yes
	NAME=br1
	IPADDR=172.21.96.2	
	NETMASK=255.255.255.0

Modify adaptor "eth0" for bridging.

	BRIDGE=br0
	TYPE="Ethernet"
	NAME="eth0"
	UUID="e485829a-8488-4466-99ba-195ac9991f00"
	DEVICE="eth0"
	ONBOOT="yes"
	BRIDGE=br0
	BOOTPROTO=none
OR

	DEVICE=bond1
	ONBOOT=yes
	TYPE=Bond
	BONDING_MASTER=yes
	BONDING_OPTS="mode=1 miimon=100 primary=eth2 updelay=120000"
	TYPE=Ethernet
	BRIDGE=br1
	BOOTPROTO=none
Then restart network service

	systemctl restart NetworkManager
	systemctl restart network

### Step 4: Create your first virtual machine
CREATE CENTOS 7.X VM

**Import qcow2 disk image can use the following command**

	virt-install --import --name galaxy \
	--memory 2048 --vcpus 2 --cpu host \
	--disk /opt/image/Galaxy-Image.qcow2,format=qcow2,bus=virtio \
	--network bridge=br0,model=virtio \
	--os-type=linux \
	--os-variant=centos7.0 \
	--graphics vnc,listen=0.0.0.0  \
	--noautoconsole

OR

	/usr/libexec/qemu-kvm -m 2048 -smp 2 --enable-kvm  -boot c -hda /opt/image/Galaxy-Image.qcow2   -vnc :1

**Install from CDROM**

创建磁盘

	qemu-img create -f qcow2 -o cluster_size=2M /opt/image/esp.qcow2 3072G
执行安装（如果已有磁盘文件不用在指定磁盘大小）
	
	virt-install \
	--virt-type=kvm \
	--name centos7 \
	--ram 2048 \
	--vcpus=1 \
	--os-variant=centos7.0 \
	--cdrom=/var/lib/libvirt/boot/CentOS-7-x86_64-Minimal-1708.iso \
	--network=bridge=br0,model=virtio \
	--graphics vnc,listen=0.0.0.0 \
	--disk path=/var/lib/libvirt/images/centos7.qcow2,size=40,bus=virtio,format=qcow2

### 配置virsh console

	echo "ttyS0" >> /etc/securetty
在/etc/grub2.conf文件中,在kernel行添加如下配置console=tty0 console=ttyS0,115200n8:

	default=0
	timeout=5
	splashimage=(hd0,0)/grub/splash.xpm.gz
	hiddenmenu
	title CentOS (2.6.32-642.4.2.el6.x86_64)
        root (hd0,0)
        kernel /vmlinuz-2.6.32-642.4.2.el6.x86_64 ro root=/dev/mapper/vg_linux-lv_root rd_NO_LUKS LANG=en_US.UTF-8 rd_LVM_LV=vg_linux/lv_swap rd_NO_MD rd_LVM_LV=vg_linux/lv_root SYSFONT=latarcyrheb-sun16 crashkernel=auto  KEYBOARDTYPE=pc KEYTABLE=us rd_NO_DM rhgb quiet console=tty0 console=ttyS0,115200n8
        initrd /initramfs-2.6.32-642.4.2.el6.x86_64.img
在/etc/inittab中添加agetty:

	S0:12345:respawn:/sbin/agetty ttyS0 115200, 1152000 xterm
重启

	reboot

## Centos挂载win共享目录
[Preparing CentOS 7 for Mounting SMB Shares](https://www.serverlab.ca/tutorials/linux/storage-file-systems-linux/mounting-smb-shares-centos-7/)

1.Install the cifs-utils package from the default CentOS yum repository.

	yum install cifs-utils
2.Next, we need an account on the CentOS server that will map to the Windows account granted permission to the SMB share, _share_library_core. We’ll create a service account named svc_library_core with a user id (UID) of 5000.

	useradd -u 5000 svc_library_core

3.We also want a group on the CentOS server that will map to the share. This group will contain all of the Linux accounts that will need access to the share. Our account will be called share_library_core and it will have a group id (gid) of 6000.

	groupadd -g 6000 share_library_core

4.Finally, add any Linux accounts that require access to the SMB share to the newly created Linux group. I have an existing account named user1 that I will add to the share_library_core group.
	
	usermod -G share_library_core -a zh

### Mounting an SMB Share
1.Create a directory to mount the SMB share into. We’ll mount the share in a directory called lib_core.

	mkdir -p /mnt/share/
2Using the mount.cifs command, mount the SMB share into lib_core using the Active Directory user account _share_library_core. We need to map the UID of our svc_library_core account (5000) and the gid of our share_library_core group (6000) to the SMB share.

	mount.cifs -o vers=2.0,user=wiwiz,pass="zhangheng2",uid=5000,gid=6000 //172.16.1.105/BaiduNetdiskDownload /mnt/share/ 
