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



