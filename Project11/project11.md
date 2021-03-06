Project 11

Updaate jenkins server tag

![alt text](./tags.png)

Create a new Repo on Github

![alt text](./tags.png)

sudo apt update

sudo apt install ansible

![alt text](./sudoansi.png)

Create a new Freestyle project ansible in Jenkins and point it to your ‘ansible-config-mgt’ repository.

![alt text](./anssy.png)




Configure Webhook in GitHub and set webhook to trigger ansible build.


![alt text](./web.png)



Configure a Post-build job to save all (**) files, like i did it in Project 9.


![alt text](./Screenshot_1.png)

![alt text](./Screenshot_2.png)

Test my setup by making some change in README.MD file in master branch and make sure that builds starts automatically and Jenkins saves the files (build artifacts) in following folder
ls /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/


![alt text](./proof.png)

BEGIN ANSIBLE DEVELOPMENT

In my ansible-config-mgt GitHub repository, created a new branch that will be used for development of a new feature.

on Vs code press, ctrl,shift ,P

Git clone

Choose clone from Github and choose the ansible repository we just created.

Make sure Git for windows installed, if not download and install on ur PC

load new terminal, pick gitbash as terminal type, will ask you to navigate to folder where ansible is located that was just cloned.

Create a new branch

![alt text](./newbranch.png)

Create a directory and name it playbooks – it will be used to store all your playbook files.

mkdir playbooks

![alt text](./playbooks.png)

Create a directory and name it inventory – it will be used to keep your hosts organised.

![alt text](./inventory.png)

Within the inventory folder, create an inventory file (.yml) for each environment (Development, Staging Testing and Production) dev, staging, uat, and prod respectively.

![alt text](./yml.png)

Lets add SSH agents

Install Open SSH with instructions in link below

https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse


Open powershell

![alt text](./ssh.png)

![alt text](./ssh2.png)

Test if you can login to NFS and other servers via ansible on terminal without pem key

![alt text](./test.png)

Go to ansible project on vs code

![alt text](./assy.png)

Update dev.yml

[nfs]
172.31.82.179 ansible_ssh_user='ec2-user'

[webservers]
172.31.92.195 ansible_ssh_user='ec2-user'
172.31.82.66 ansible_ssh_user='ec2-user'

[db]
172.31.89.191 ansible_ssh_user='ec2-user' 

[lb]
172.31.19.152 ansible_ssh_user='ubuntu'

Update common.yml

Commit your code into GitHub:

use git commands to add, commit and push your branch to GitHub.
git status

git add <selected files>

git commit -m "commit message"
Create a Pull request (PR)

Wear a hat of another developer for a second, and act as a reviewer.

If the reviewer is happy with your new feature development, merge the code to the master branch.

Head back on your terminal, checkout from the feature branch into the master, and pull down the latest changes.

![alt text](./pull.png)

![alt text](./pull2.png)

Check jenkins

![alt text](./jenkins.png)


Once your code changes appear in master branch – Jenkins will do its job and save all the files (build artifacts) to /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/ directory on Jenkins-Ansible server.


![alt text](./see.png)


ANSIBLE TEST


Connect to ansible /jenkins server

![alt text](./ubuntu.png)


Some problems came up.

Kept timing out when tried to connect. Troubleshhot.

Checked ec2 and found out that the jenkins server had an error.

![alt text](./j.png)

So i stopped and started the instance.

This gave the instance a new ip. So i decided to get a elastic ip and attach it to the jenkins-ansible server.

![alt text](./kk.png)
![alt text](./k.png)



So i had to change the config in webhook in github ansible project


![alt text](./kkk.png)

And do another build.

Now i opened the remote server. It faiiled 4 times and i kept clicking retry.

![alt text](./remote.png)

Now run

ansible-playbook -i /var/lib/jenkins/jobs/ansible/builds/9/archive/inventory/dev.yml /var/lib/jenkins/jobs/ansible/builds/9/archive/playbooks/common.yml 

It failed

![alt text](./stuck.png)

I checked the dev.yml

![alt text](./user.png)

The db server is ubuntu and not redhat, so i corrected the ssh user in dev.yml to ubuntu.

I also noticed that the db server is listed as redhat instead of ubuntu on so i corrected that.

It worked

![alt text](./success.png)


Confirm wireshark install on the servers.

Webserver 2

![alt text](./webserver2.png)


Webserver 1

![alt text](./wire1.png)

Nfs server

![alt text](./nfs.png)

Db server

![alt text](./db.png)

LB server

![alt text](./lb.png)

Go back to git terminal

git checkout prj-11

Run the commands

Update the playbook

![alt text](./playbook-updated.png)

Sync with github

Run the playbook

ansible-playbook -i /var/lib/jenkins/jobs/ansible/builds/10/archive/inventory/dev.yml /var/lib/jenkins/jobs/ansible/builds/10/archive/playbooks/common.yml 


Success

Check if it worked on NFS server

![alt text](./final.png)










