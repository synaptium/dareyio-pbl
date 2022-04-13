 Project 5

 Install 2 Ec2 instance. 1 is Sever for database, 2nd is client machine

 ![alt text](./Screenshot_8.png)

 Server

 `sudo mysql_secure_installation`

  ![alt text](./Screenshot_1.png)

  `CREATE DATABASE test_db;`

  ![alt text](./Screenshot_2.png)

  `GRANT ALL ON test_db.* TO 'remote_user'@'%' WITH GRANT OPTION;`

   ![alt text](./Screenshot_3.png)

   `FLUSH PRIVILEGES;`
   ![alt text](./Screenshot_4.png)

   `sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf`
   `sudo systemctl restart mysql`

 ![alt text](./Screenshot_5.png)

 CLIENT

`sudo apt update`

Install mysql client

`ip addr show`

![alt text](./Screenshot_6.png)

Add client private ip to server security inbound rules to allow access to mysql

![alt text](./Screenshot_7.png)


sudo mysql -u remote_user -h 172.31.3.117 -p

![alt text](./Screenshot_8.png)