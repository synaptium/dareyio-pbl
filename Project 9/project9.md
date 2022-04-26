Project 9

Create Ubuntu Ec2 Instance

![alt text](./instance.png)

sudo apt update

![alt text](./update.png)

Install JDK

sudo apt install default-jdk-headless

![alt text](./jenkins.png)

Install jenkins

wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \
    /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt-get install jenkins

![alt text](./jenkins2.png)

![alt text](./jj.png)

Load site from port 8080 on browser

![alt text](./8080.png)

Retrieve password

sudo cat /var/lib/jenkins/secrets/initialAdminPassword

Login

![alt text](./jk.png)

![alt text](./jh.png)

Go to github and created a REPO called tooling

Configure webhook in its settings

![alt text](./hook.png)

Showed error.

Edited readme file and saved.

Make sure to use public ip of instance not provate

Make sure url is correct

http://35.169.116.102:8080/github-webhook/

Choose json

Edited the read me again and showed sucess mark

![alt text](./webhook.png)

Build again and success

![alt text](./hooker.png)

Install Plugin "Publish Over SSH".

Manage jenkins

manage plugins

Install plugins

![alt text](./overssh.png)

On NFS server mount apps directory

![alt text](./mount.png)

over SSH plugin configuration section

![alt text](./key.png)


![alt text](./key2.png)


To make sure that the files in /mnt/apps have been udated – connect via SSH/Putty to your NFS server and check README.MD file

cat /mnt/apps/README.md

![alt text](./last.png)

DONE!!!