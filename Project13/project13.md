Log in to gitbash terminal with ansible mgt folder

Create a new branch called dynamic-assignments

git checkout -b dynamic-assignments

![alt text](./createbranch.png)

Create a file called en-vars.yml under it

Now create a folder in ansible call env-sars and create the following files under it.

![alt text](./envars.png)

in the en-vars.yml, paste the following codes.

---
- name: collate variables from env specific file, if it exists
  hosts: all
  tasks:
    - name: looping through list of available files
      include_vars: "{{ item }}"
      with_first_found:
        - files:
            - dev.yml
            - stage.yml
            - prod.yml
            - uat.yml
          paths:
            - "{{ playbook_dir }}/../env-vars"
      tags:
        - always

Now since i stopped and started my instaces in project 12, the jenkins ansible server Public i[ has changed.

So i updated ip in ansible repo on github webhook.

then did a push from vscode to git hub to cause a trigger in jenkins and therefore my jenkins ansible server.

![alt text](./see.png)

![alt text](./see2.png)

Login into the ssh.

Change directory to ansible-config-artifact

cd roles

run git init

run ansible-galaxy install geerlingguy.mysql

rename the folder rename the folder to mysql with mv geerlingguy.mysql/ mysql


![alt text](./mysql.png)

![alt text](./mysql2.png)

Download the roles folder from remote server to local pc and copy into ansible folder in local pc.

![alt text](./roles.png)

Now its time to edit the main.yml file in roles/mysql/default folder


![alt text](./editmysql.png)

Inside roles directory create new roles Nginx and Apache

and run commands

ansible-galaxy install geerlingguy.nginx

and

ansible-galaxy install geerlingguy.apache

Rename the servers as in the below images

![alt text](./rename.png)

Download the new nginix and apache folders and copy into ansible project directory on my laptop

Go to ubuntu remote server and navigate to ansible-config-artifact/roles/ngnix/defaults/main.yml file

Tweak the following

![alt text](./ngnix.png)

![alt text](./ngnix2.png)

Go to on remote server

ansible-config-artifact/roles/ngnix/tasks/main.yml and edit below


![alt text](./ngnix3.png)

![alt text](./ngnix4.png)










