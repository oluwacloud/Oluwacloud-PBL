# PROJECT 2: LEMP STACK IMPLEMENTATION
## Step 1 - Installing the Nginx Web Server
The below cmdlet shows how to use the apt package manager to install Nginx 

```
sudo apt update
sudo apt install nginx
```

![Project 2, 1](https://user-images.githubusercontent.com/68804488/150544221-548b31d7-a585-446a-9155-fe5afa020e54.png)

![Project 2, 2](https://user-images.githubusercontent.com/68804488/150544675-020ed500-861e-410b-807f-1903ac50e830.png)


The below cmdlet is to verify that nginx was successfully installed and is running as a service in Ubuntu

```
sudo systemctl status nginx
```
![Project 2, 3](https://user-images.githubusercontent.com/68804488/150545322-9dd468e6-4e37-47a6-9bd5-a9f114f6dfb4.png)

Opened TCP port 80 which is default port that web brousers use to access web pages in the Internet.

Also opened TCP port 22 open by default on AWS EC2 machine to access it via SSH, and added a rule to EC2 configuration to open inbound connection through port 80

![](https://user-images.githubusercontent.com/68804488/149380714-ca2117af-6d64-4f57-88ee-d8b9f9838f3e.png)

To test that the Nginx server can respond to request from the internet, I opened a web browser and accessed the url: http://54.196.15.29:80

![Project 2, 4](https://user-images.githubusercontent.com/68804488/150546174-cec70615-a5fb-4fd0-a36e-bdffacc1a814.png)

## Step 2 - Installing mysql

The below cmdlet installs the mysql in the server
```
sudo apt install mysql-server
```

![Project 2, 5](https://user-images.githubusercontent.com/68804488/150552690-ac3ce712-c330-4936-97a6-05a8afc528af.png)
![Project 2, 6](https://user-images.githubusercontent.com/68804488/150561430-982f0dff-89c1-41b5-b9e3-5661111ef93b.png)

![Project 2, 7](https://user-images.githubusercontent.com/68804488/150561469-158624ff-b0ba-468c-86cd-ecff23e0bc3b.png)
![Project 2, 8](https://user-images.githubusercontent.com/68804488/150561553-48aedd27-8b9f-4abc-84b7-1cdb2bfba265.png)

The below cmdlet runs a security script that removes some insecure default settings and lock down access to the database system
```
sudo mysql_secure_installation
```
![Project 2, 9](https://user-images.githubusercontent.com/68804488/150562227-e10c69b4-b451-4b4a-8589-7f852c60e904.png)


## Step 3 Installing php

The below cmdlet install packages *php-fpm* to tell Nginx to pass PHP requests and *php-mysql* for PHP to communicate with MySQL-based databases.

```
sudo apt install php-fpm php-mysql
```

![Project 2, 11](https://user-images.githubusercontent.com/68804488/150567382-a453127b-bae5-46a2-8ebd-82cf78ecb546.png)  

![Project 2, 12](https://user-images.githubusercontent.com/68804488/150568567-9a9021f5-8281-4df2-b20d-bb95ace14928.png)


**PHP components are installed**

## Step 4: Configuring nginx to use php processor

The below cmdlet creates the root web directory for your_domain as follows:

```
sudo mkdir /var/www/projectLEMP


