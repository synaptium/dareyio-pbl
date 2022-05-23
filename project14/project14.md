Project 14

Launch a new red hat instance, with t2 medium because you will be sinatlling ansbile and jenkins. Need more space.

Connect via remote host and open the server. To do this, i edited the remote host config file 


![alt text](./remote.png)


Run sudo yum install git -y to install git

![alt text](./gitty.png)


Clone Davids repo

![alt text](./clonegit.png)

Install jenkins

https://www.jenkins.io/doc/book/installing/

Choose OS and wheher ubutu or redhat and see info on how to install

Switch to root with sudo su

and paste

sudo wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat-stable/jenkins.repo

For red hat installation of jenkins


Got this error, because wget is not installed

![alt text](./wget.png)

So found this page on google on how to install wgt on redhat 8

https://www.cyberciti.biz/faq/yum-install-wget-redhat-cetos-rhel-7/

So installed wget and ran my jenkins install again.

![alt text](./wgetok.png)

Next stage

sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key

yum install -y dnf-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm

intall java

sudo yum install java-11-openjdk-devel -y

Run the following - we do this to make sure the bash profile is always started when the server is restarted.

##### open the bash profile 
vi .bash_profile 

##### paste the below in the bash profile
export JAVA_HOME=$(dirname $(dirname $(readlink $(readlink $(which javac)))))
export PATH=$PATH:$JAVA_HOME/bin
export CLASSPATH=.:$JAVA_HOME/jre/lib:$JAVA_HOME/lib:$JAVA_HOME/lib/tools.jar

##### reload the bash profile
source ~/.bash_profile

intsall jekins

sudo yum install jenkins

sudo systemctl start jenkins

sudo systemctl enable jenkins

sudo systemctl status jenkins

![alt text](./wgetok.png)

sudo systemctl daemon-reload

Check jenkins on browser

![alt text](./jenkinsinstalled.png)

do a 

sudo cat /var/lib/jenkins/secrets/initialAdminPassword

to get initial passsword and login with it

Install sugegsted plugins

Enter admin details

Now install blue ocean plugin


![alt text](./blueocean.png)

![alt text](./blue.png)

In vscode got to ansible folder and create a folder called deploy and create a file under it called Jekinsfile

push to git hub and do a git pull on remote server

Now configure the ansible project in jenkins

Under build configuration

change to deploy/Jenkins file

Now go to dashboard, click on project name and click on main

The click on build now


![alt text](./buildnow.png)

Click on open blue ocean


Now create a new branch called  feature/jenkinspipeline-stages  with git checkout -b feature/jenkinspipeline-stages

Do this in git bach terminal on vscode

On git bash do 

git add .

git commit -m "add jenkins file"

git push

It did a pull merge request on github....

so i merged

Go to jenkins, under ansible project, click scan repository now

Jenkins is seeing the new branch


![alt text](./jenkinsee.png)

![alt text](./jenkinsee2.png)

Now edit the jenkin file

add the following code

   pipeline {
    agent any

   stages {
     stage("Initial cleanup") {
           steps {
             dir("${WORKSPACE}") {
               deleteDir()
            }
           }
         }
     stage('Build') {
       steps {
         script {
           sh 'echo "Building Stage"'
         }
       }
     }

     stage('Test') {
       steps {
         script {
           sh 'echo "Testing Stage"'
         }
       }
     }

     stage('Package'){
       steps {
         script {
	       sh 'echo "Packaging App" '
	     }
       }
     }  

     stage('Deploy'){
       steps {
         script {
	       sh 'echo "Deploying to Dev" '
	     }
       }

    stage("clean up")
      steps {
        cleanWs()
            }
      }
    }  
}

It failed

![alt text](./failed.png)

So the issue was that 

![alt text](./curly.png)

Correct code

   pipeline {
    agent any

   stages {
     stage("Initial cleanup") {
           steps {
             dir("${WORKSPACE}") {
               deleteDir()
            }
           }
         }

     stage('Build') {
       steps {
         script {
           sh 'echo "Building Stage"'
         }
       }
     }

     stage('Test') {
       steps {
         script {
           sh 'echo "Testing Stage"'
         }
       }
     }

     stage('Package'){
       steps {
         script {
	       sh 'echo "Packaging App" '
	     }
       }
     } 

     stage('Deploy'){
       steps {
         script {
	       sh 'echo "Deploying to Dev" '
	     }
       }
     }

     stage("clean up"){
       steps {
        cleanWs()
      }
     }
      
     }
}

It works


![alt text](./codework.png)


Now you need to do a merge request, to update main branch with data from feature/jenkinspipeline-stages branch on Github

To do this if you go to ansible repo on Github and go to main branch, you should see a pull request or go to feature/jenkinspipeline-stages branch and click on contribute and choose open a request, then click pull request, then merge pull request, then confirm merge.

![alt text](./mainbuild.png)

So because of the merge, it triggers main build. It succeeded.

![alt text](./main.png)

Now install ansible on jenkins

sudo yum install ansible -y

Verify ansible is installed.

![alt text](./verify.png)

There are some ansible dependencies that need to be installed.

Do a sudo su to switch to root

python3 -m pip install --upgrade setuptools
python3 -m pip install --upgrade pip
python3 -m pip install PyMySQL
python3 -m pip install mysql-connector-python
python3 -m pip install psycopg2==2.7.5 --ignore-installed

ansible-galaxy collection install community.postgresql


Do it one by one

Install ansible on Jenkins

![alt text](./ansi.png)

Run Ansible playbook

Launch 2 instances

Ngnix- red hat and Db - ubuntu


Remove all the old code in jenkins file and add below

This is the ansible playbook

pipeline{
  agent any
  
  environment {
      ANSIBLE_CONFIG="${WORKSPACE}/deploy/ansible.cfg"
    }

  stages {   
    stage("Initial cleanup") {
          steps {
            dir("${WORKSPACE}") {
              deleteDir()
            }
          }
        }

      stage('Checkout SCM') {
         steps{
            git branch: 'feature/jenkinspipeline-stages', url: 'https://github.com/synaptium/ansible-config-mgt.git'
         }
       }
       stage('Prepare Ansible For Execution') {
        steps {
          sh 'echo ${WORKSPACE}' 
          sh 'sed -i "3 a roles_path=${WORKSPACE}/roles" ${WORKSPACE}/deploy/ansible.cfg'  
        }
       }

      stage('Run Ansible playbook') {
        steps {
           ansiblePlaybook become: true, colorized: true, credentialsId: 'private-key', disableHostKeyChecking: true, installation: 'ansible', inventory: 'inventory/${inventory}', playbook: 'playbooks/site.yml'
         }
      }

      stage('Clean Workspace after build'){
        steps{
          cleanWs(cleanWhenAborted: true, cleanWhenFailure: true, cleanWhenNotBuilt: true, cleanWhenUnstable: true, deleteDirs: true)
        }
      }
   }

}


Jenkins needs to ssh into the instanes so we need to pass the keys to jenkins

Now go to jenkins, click on manage jekins, credentials, global, add credentials 

Choose ssh user with private key.

I got a crumb error.

To fx this go to jenkins manage, install plugin, install crumb issuer plugin.

This worked for me

In the beginning of davids video at about 21:50, he showd other solutions

Now enter thiese settings below for setup of credentials

![alt text](./jenkinskey.png)

![alt text](./jenkinskey2.png)

Now go to dashboard, click on ansible project, click pieline syntax

Its all empty

Now go back to dashboard, manage jenkins, global tool config

scroll down to ansible

Name  - ansible

Path  is

Got to server and do a which ansible

copy the path and paste

/usr/bin/


Go back to pipeline synatax

choose settings below

![alt text](./config.png)

![alt text](./config2.png)

![alt text](./config3.png)


Click generate

Copy output

















