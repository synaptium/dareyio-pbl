Project 12

Install Ubuntu Instance with Scripts loaded to run on deployment.

#!/usr/bin/bash
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee \
 /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
    https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
    /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update -y
  sudo apt-get install fontconfig openjdk-11-jre -y
  sudo apt-get install jenkins

  
  Add the ubuntun server pem key to ssh agent on local pc


![alt text](./sshadd.png)

Confirm that Jenkins is inatlled


![alt text](./seejenkins.png)


![alt text](./jjp.png)


Run sudo cat /var/lib/jenkins/secrets/initialAdminPassword

to get scret key

Pass as pssword.

install suggested plugins

![alt text](./plugins.png)


Update Webhook in github.

Open ansible repo and go to settings - update ip.


![alt text](./webby.png)

Go to Jenkins and do a build.

![alt text](./build.png)

Go to your Jenkins-Ansible server and create a new directory called ansible-config-artifact – we will store there all artifacts after each build.

sudo mkdir /home/ubuntu/ansible-config-artifact

Change Dir Permissions

sudo chmod -R 0777 /home/ubuntu/ansible-config-artifact

or 

sudo chmod -R 0777 /home

Go to Jenkins web console -> Manage Jenkins -> Manage Plugins -> on Available tab search for Copy Artifact and install this plugin without restarting Jenkins

![alt text](./artifact.png)

Create a new Freestyle project and name it save_artifacts.


![alt text](./save.png)


Configured with following things

![alt text](./trigger.png)

name should be project12 not ansible in our case.

Edit Readme file in Ansible Repo on github

check if push worked?

Success

![alt text](./7.png)


![alt text](./77.png)

Create static-assignements folder in ansible repo
and create site.yml under playboook folder


![alt text](./see.png)

Move common.yml into static assignments folder


![alt text](./move.png)

Push to github.

Now connect to remote host of jenkins-ansible server.

I did was not seeing the full folders and files i pushed.

So i went back to my project 12 config in jenkins

I changed the branch to main, since vscode pushed to main branch.

How do i know?

I went to github and saw that the main branch had the files i pushed and master was outdated.

To test i edited the README.md file on main branch and saved and lo and behold check project 12 in jenkins and a build was triggered successfully.

Now connect to remote host. Files were showing now.

I ran

ansible all -m ping

and it said ansible not installed.

So i went back to other vscode window to install ansible.

This happened because i lost my aws account and created EC2 instaces afresh.

![alt text](./ansicbleconfig.png)

![alt text](./ass.png)


