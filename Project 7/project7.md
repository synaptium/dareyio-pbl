Project 7

Instances created on AWS. 

![alt text](./instancces-launched.png)

3 Volumes created for NFS server and attached to NFS server

![alt text](./volumes-for-nfs.png)

Logged in to NFS machine on terminal

![alt text](./logged-to-nfs.png)



List Blocks

![alt text](./list-blocks.png)



Gdisk to create single partition on each disks

 ![alt text](./partitionsdone.png)

 Install LVM package

 sudo yum install lvm2 -y

  ![alt text](./lvm.png)

Check for available partitions

  sudo lvmdiskscan

  ![alt text](./lvmscan.png)

  Create physical volume

  sudo pvcreate /dev/xvdf1

  sudo pvcreate /dev/xvdg1

  sudo pvcreate /dev/xvdh1

  ![alt text](./pvcreate.png)

Check if volumes are present

  sudo pvs

![alt text](./sudopvs.png)

Combine all 3 partitions into 1

sudo vgcreate webdata-vg /dev/xvdh1 /dev/xvdg1 /dev/xvdf1

![alt text](./combine.png)

check for the status of combination

sudo vgs

![alt text](./sudogvs.png)

Create 3 Logical Volumes. lv-opt lv-apps, and lv-logs

sudo lvcreate -n lv-apps -L 9G webdata-vg


![alt text](./logicvolumes.png)

Verify setup

sudo vgdisplay -v #view complete setup - VG, PV, and LV

![alt text](./verifyvolume.png)

Formart disk as xfs

sudo mkfs -t xfs /dev/webdata-vg/lv-apps

![alt text](./xfs.png)

Create mount points

![alt text](./mount.png)

sudo yum -y update

![alt text](./yamupdate.png)

Switched to DATABASE SERVER

sudo apt install mysql-server -y

Threw errror

![alt text](./sudoerror.png)

sudo apt update

![alt text](./sudoaptupdate.png)

Install mysql server

![alt text](./sqlagain.png)

sudo mysql

![alt text](./mysqlin.png)

create database tooling;

![alt text](./createdatabase.png)

Create user

create user 'webaccess'@'172.31.80.0/20' identified by 'password';

![alt text](./createuser.png)

Grant webaccess full rights on tooling database

grant all privileges on tooling.* to 'webaccess'@'172.31.80.0/20';

![alt text](./toolingaccess.png)


Back to NFS server

sudo yum install nfs-utils -y

![alt text](./nfs-utils.png)



sudo systemctl start nfs-server.service

sudo systemctl enable nfs-server.service

sudo systemctl status nfs-server.service

![alt text](./nfsrunning.png)


sudo chown -R nobody: /mnt/apps
sudo chown -R nobody: /mnt/logs
sudo chown -R nobody: /mnt/opt

sudo chmod -R 777 /mnt/apps
sudo chmod -R 777 /mnt/logs
sudo chmod -R 777 /mnt/opt

sudo systemctl restart nfs-server.service


![alt text](./restart.png)


![alt text](./status.png)

Configure access to NFS for clients within the same subnet 

sudo vi /etc/exports

/mnt/apps 172.31.80.0/20(rw,sync,no_all_squash,no_root_squash)
/mnt/logs 172.31.80.0/20(rw,sync,no_all_squash,no_root_squash)
/mnt/opt 172.31.80.0/20(rw,sync,no_all_squash,no_root_squash)

Esc + :wq!

sudo exportfs -arv

![alt text](./exports.png)

SWITCH TO SERVER 1

Mount /var/www/ and target the NFS server’s export for apps

sudo mount -t nfs -o rw,nosuid 172.31.82.179:/mnt/apps /var/www

![alt text](./mountnfs.png)


Install Apache

sudo yum install httpd -y


![alt text](./apache.png)


Install git

sudo yum install git


![alt text](./install-git.png)

Fork Repository

git clone https://github.com/darey-io/tooling.git

![alt text](./fork.png)

sudo systemctl start httpd

![alt text](./start-apache-service.png)

Load site

![alt text](./load-site.png)

Update the website’s configuration to connect to the database

sudo vi /var/www/html/functions.php

![alt text](./functions.png)

sudo yum install mysql

![alt text](./install-m.png)

SWITCH TO MYSQL SERVER

Edit mysql.cnf to change bind address and mysql bind address from
127.0.0.1 to 0.0.0.0

sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf

Restart mysql

sudo systemctl restart mysql
sudo systemctl status mysql

![alt text](./mysql-re.png)

SWITCH BACK TO SERVER 1

go to tooling folder

and in mysql run

mysql -h 172.31.89.191 -u webaccess -p tooling < tooling-db.sql

Done!


![alt text](./finally.png)

