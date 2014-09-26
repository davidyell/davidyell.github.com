---
layout: post
title: "Sharing folders from a Mac to Ubuntu using Samba"
description: "Loading my work folder at home on Ubuntu."
comments: true
categories: [samba, ubuntu]
date: 2013-01-18 09:43:25
---

So due to snow I had to work from home. I needed to mount my work files over the VPN so that I could use my work Mac as my server. This would save me loads of time, allowing me to edit the files remotely and view them using my Macs nginx server.  

##Using Samba / CIFS
###Setup
For this method you'll need to enable file sharing on the Mac locally in **System Preferences** > **Sharing** and enable **File Sharing**. Then you'll want to share a folder, probably your home folder, `/Users/david`. Hit **Options** and enable sharing with SMB. Be sure to click the account that you want to use, and enter your password.  

###Connecting to the share
In Ubuntu or Windows, you should be able to connect to the folder using the IP address of your computer, in the format of `smb://192.168.0.1` then enter your local machine login.  

###Mounting the share
Check to make sure that you have `cifs` installed by looking for `cifs-utils` package.  
```bash
dpkg --get-selections cifs-utils
```  
If not, you'll need to install it. `sudo apt-get install cifs-utils`.  

In terminal  
```bash
sudo mount -t cifs //server/share -o 'username=david,password=mypassword,rw,nounix,noserverinfo,sec=ntlmssp,file_mode=0777,dir_mode=0777' /mnt/david_mac
```

##Using NFS
###Setup
Firstly you need to edit the `/etc/exports` on the Mac to add the folder that you want to share. Something like `"/Users/david" -alldirs -mapall=501` will share the home folder to everyone on your network, fine for internal mounting.  

Next we need to make sure that the file is correct and nfsd is running.  

```bash
$ showmount -e
$ sudo nfsd checkexports
$ sudo nfsd status
$ sudo nfsd restart
$ showmount -e
```

###Mount the share
`sudo mount -t nfs -o 'proto=tcp,port=2049,nolock' hostname:/Users/david /mnt/david_mac/`

##Done
Have a celebratory cup of tea. You deserve it.