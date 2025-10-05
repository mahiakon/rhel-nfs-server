# rhel-nfs-server
NFS Server Configuration Guide
Overview
This document outlines the steps taken to configure an NFS (Network File System) server on a CentOS-based system (e.g., RHEL) within a VMware virtual machine. The setup allows Windows clients to access the shared directory.
Prerequisites

CentOS/RHEL installed on a VMware VM.
Git installed for version control.
Network connectivity between the VM and Windows client.
Administrative (root) access to the VM.

Configuration Steps
1. Initial Setup

Cloned the repository:git clone https://github.com/mahiakon/rhel-nfs-server.git
cd rhel-nfs-server


Initialized Git repository:git init
git remote add origin https://github.com/mahiakon/rhel-nfs-server.git



2. NFS Server Installation

Installed NFS utilities:yum install nfs-utils -y


Started and enabled NFS service:systemctl start nfs-server
systemctl enable nfs-server



3. Directory and Export Configuration

Created shared directory:mkdir /rhel-nfs-server
chmod -R 755 /rhel-nfs-server


Configured /etc/exports:/rhel-nfs-server *(rw,sync,no_subtree_check)


Applied export changes:exportfs -ra



4. Firewall Configuration

Opened necessary ports for NFS:firewall-cmd --permanent --add-service=nfs
firewall-cmd --permanent --add-service=mountd
firewall-cmd --permanent --add-service=rpc-bind
firewall-cmd --reload



5. SELinux Adjustments

Set SELinux context for the shared directory:chcon -R -t samba_share_t /rhel-nfs-server
setsebool -P samba_export_all_ro on
setsebool -P samba_export_all_rw on



6. Windows Client Access

Installed NFS client on Windows (via "Turn Windows features on or off" > "Services for NFS").
Mounted the NFS share:mount -o anon 192.168.30.128:/rhel-nfs-server Z:


Verified connection: Z: drive successfully connected.

7. Git Version Control

Staged and committed changes:git add .
git commit -m "NFS server setup with exports and firewall configuration"


Pushed to GitHub (ensure main or master branch is set):git push -u origin main



Troubleshooting

Check NFS status: systemctl status nfs-server
View logs: cat /var/log/messages
Verify network: ping 192.168.30.128 from Windows.
Ensure firewall rules are active: firewall-cmd --list-all

Notes

IP address 192.168.30.128 is used as an example; replace with your VM's IP.
Adjust paths and permissions as per your setup.

Author

Atikur Rahman
Email: atikbs106@gmail.com
Date: October 06, 2025
