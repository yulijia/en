---
published: ture
layout: post
title: "How to set up Samba on CentOS"
author: Yu
categories: HowTo
tags:
- Samba
- CentOS
---


Samba is the standard Windows interoperability suite of programs for Linux and Unix. 
People can browse a server directory like use Windows OS on their own computer.
It also could avoid the `scp` step if you want to download and open a text file in the server,  yes, you could open any files on the server and edit it like on your own computer.

Here I will introduce the steps to set up Samba on CentOS in a home network. There may be a problem if someone wants to use this Samba function on Windows 10 systems with a server that support different version of the Samba protocol.
My laptop OS is Fedora, and the server OS is CentOS.

1. Install Samba on **local computer** and **server**, eg: `yum install samba samba-client samba-client-libs`.
2. Start the protocol on server, `sudo systemctl start smb.service`.
  - The `smbd` service provides file sharing and printing services and listens on TCP ports 139 and 445. At here, for personal reason, I didn't want to start the `nmbd` server, which provides NetBIOS over IP naming services to clients and listens on UDP port 137.
3. Set up firewall
```bash
firewall-cmd --list-services
firewall-cmd --list-ports
firewall-cmd --permanent --zone=public --add-service=samba
```
4. I want to use the `/home/yulijia` directory via Samba, so the Samba configuration file (`/etc/samba/smb.conf`) could be:
```bash
[homes]
	comment = Home Directories
	valid users = %S, %D%w%S
	browseable = Yes
	read only = No
	create mask = 0700
        directory mask = 0700
	inherit acls = Yes
```
Once done, please restart Samba services, `systemctl restart smb.service`
5. Connect from your local machine.
Because I use Fedora, so I could open the Nautilus to add a new `connect to server` link (`smb://192.168.0.111:139/`), then connect to the server, select Registered User, enter your server username and password.

![connect_to_samba](https://i.imgur.com/esdDgUE.png)

![fill_in_username_and_password](https://i.imgur.com/EAe24pn.png)

Then the files on the Samba server will be shown.

Note: the default port of Samba service is `139`.


Reference:

[1] [How to Install and Configure Samba on CentOS 7](https://linuxize.com/post/how-to-install-and-configure-samba-on-centos-7/)
