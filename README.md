NFS Server Setup Guide
Overview
A quick guide to set up an NFS server on CentOS in VMware and access from Windows.
Prerequisites

CentOS/RHEL on VMware.
Git installed.
Root access.

Steps
1. Setup

Clone repo: git clone https://github.com/mahiakon/rhel-nfs-server.git
Enter dir: cd rhel-nfs-server
Init Git: git init
Add remote: git remote add origin https://github.com/mahiakon/rhel-nfs-server.git

2. Install NFS

Install: yum install nfs-utils -y
Start: systemctl start nfs-server
Enable: systemctl enable nfs-server

3. Configure Share

Make dir: mkdir /rhel-nfs-server
Set perms: chmod -R 755 /rhel-nfs-server
Edit /etc/exports: echo "/rhel-nfs-server *(rw,sync,no_subtree_check)" >> /etc/exports
Apply: exportfs -ra

4. Firewall

Add services: firewall-cmd --permanent --add-service=nfs
Add more: firewall-cmd --permanent --add-service=mountd
Add RPC: firewall-cmd --permanent --add-service=rpc-bind
Reload: firewall-cmd --reload

5. SELinux

Set context: chcon -R -t samba_share_t /rhel-nfs-server
Enable ro/rw: setsebool -P samba_export_all_ro on
Enable rw: setsebool -P samba_export_all_rw on

6. Windows Access

Mount: mount -o anon 192.168.30.128:/rhel-nfs-server Z:

7. Git Push

Add files: git add .
Commit: git commit -m "NFS setup"
Push: git push -u origin main

Troubleshooting

Check NFS: systemctl status nfs-server
View logs: cat /var/log/messages
Ping: ping 192.168.30.128

Notes

Use your VM IP instead of 192.168.30.128.
Author: Atikur Rahman, Email: atikbs106@gmail.com
Date: October 06, 2025
