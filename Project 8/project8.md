Install Ec2 instance

![alt text](./instance.png)

Open Port 80

![alt text](./port.png)

Install apache load balancer

sudo apt install apache2 -y

![alt text](./load.png)

Configure Load Balancing

sudo vi /etc/apache2/sites-available/000-default.conf

![alt text](./ll.png)

Verify that our configuration works – try to access your LB’s public IP address or Public DNS name from your browser

![alt text](./index.png)

Configure Local DNS Names Resolution

sudo vi /etc/hosts

![alt text](./web.png)

 curl http://Web1 or curl http://Web2 

 ![alt text](./curl.png)