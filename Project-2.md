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

The below cmdlet creates the root web directory for my_domain

```
sudo mkdir /var/www/projectLEMP
```
Assigned ownership of the directory with the $USER environment variable, which referenced current system user:

```
sudo chown -R $USER:$USER /var/www/projectLEMP
```
Opened a new configuration file in Nginx’s sites-available directory using nano:

```
sudo nano /etc/nginx/sites-available/projectLEMP
```
Inputed the following bare-bones configuration:

```
#/etc/nginx/sites-available/projectLEMP

server {
    listen 80;
    server_name projectLEMP www.projectLEMP;
    root /var/www/projectLEMP;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

}
```
Activated the configuration by linking to the config file from Nginx’s sites-enabled directory:

```
sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/
```
It will tell Nginx to use the configuration next time it is reloaded. 

Test configuration for syntax errors using:

```
sudo nginx -t
```
Disabled default Nginx host that is currently configured to listen on port 80, for it run using:

```
sudo unlink /etc/nginx/sites-enabled/default
```
Reloaded Nginx to apply the changes:

```
sudo systemctl reload nginx
```
Created an index.html file in the web root /var/www/projectLEMP to test that the new server block works as expected:

```
sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html
```

![Project 2, 13](https://user-images.githubusercontent.com/68804488/150671407-8dd4f7a7-0e01-4db3-adff-b2b557c75374.png)

The **'echo'** command written in the index.html file can be seen, indicating that the Nginx site is working as expected.

![Project 2, 14](https://user-images.githubusercontent.com/68804488/150672306-65e4e5f9-1436-4a21-9988-db3c1d4b3ae2.png)

## Step 5 - Testing php with nginx

Below cmdlet is to test to validate that Nginx can correctly hand .php files off to PHP processor

```
sudo nano /var/www/projectLEMP/info.php
```
![Project 2, 15](https://user-images.githubusercontent.com/68804488/150672620-029a6462-f697-441d-965a-117043401a3f.png)

Accessing this page in web browser by visiting the domain name or public IP address that was set up in the Nginx configuration file, followed by /info.php:

```
http://54.196.15.29/info.php
```
![Project 2, 16](https://user-images.githubusercontent.com/68804488/150672736-3b1e948a-ceec-4f36-b755-45776049596a.png)

## Step 6 - Retrieving data from MySQL database with PHP

Will create a database named ***example_database*** and a user named ***example_user***

First, connected to the MySQL console using the root account:

```
sudo mysql
```

![Project 2, 17](https://user-images.githubusercontent.com/68804488/150674752-a42768d2-240e-463a-9b87-2e76906ffd56.png)

To create a new database, ran the following command from the MySQL console:

```
mysql> CREATE DATABASE `example_database`;
```
The following command creates a new user named *example_user*, using mysql_native_password as default authentication method. Defined this user’s password as *password*

```
mysql>  CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password';
```
Gave this user permission over the *example_database* database:

```
mysql> GRANT ALL ON example_database.* TO 'example_user'@'%';
```
This will give the **example_user** user full privileges over the **example_database** database, while preventing this user from creating or modifying other databases on the server.

![Project 2, 18](https://user-images.githubusercontent.com/68804488/150674807-45f2d47b-1896-45a6-8d13-de3124258dfa.png)

```
mysql -u example_user -p
```
The below cmdlet confirms that there's access to the *example_database* database:
```
mysql> SHOW DATABASES;
```

![Project 2, 19](https://user-images.githubusercontent.com/68804488/150676927-a16d08df-40a2-4e32-9916-9abc524daf5e.png)

Created a test table named **todo_list** by running the following statement:

```
CREATE TABLE example_database.todo_list (
mysql>     item_id INT AUTO_INCREMENT,
mysql>     content VARCHAR(255),
mysql>     PRIMARY KEY(item_id)
mysql> );
```
Repeated the next command a few times, using different VALUES:

```
mysql> INSERT INTO example_database.todo_list (content) VALUES ("My first important item");
```

To confirm that the data was successfully saved to the table, ran:

```
mysql>  SELECT * FROM example_database.todo_list;
```
![Project 2, 21](https://user-images.githubusercontent.com/68804488/150677136-5d62a584-f25f-48fe-b6d2-cc2c9a0cf40c.png)

Exited by running:

```
mysql> exit
```

The following PHP script connects to the MySQL database and queries for the content of the **todo_list** table, displays the results in a list. Edited the *todo_list.php* script: 

```
<?php
$user = "example_user";
$password = "password";
$database = "example_database";
$table = "todo_list";

try {
  $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
  echo "<h2>TODO</h2><ol>";
  foreach($db->query("SELECT content FROM $table") as $row) {
    echo "<li>" . $row['content'] . "</li>";
  }
  echo "</ol>";
} catch (PDOException $e) {
    print "Error!: " . $e->getMessage() . "<br/>";
    die();
}
```

![Project 2, 22](https://user-images.githubusercontent.com/68804488/150677562-2bc4664c-46e5-482b-b2d1-c6af1c216bd1.png)

Page can be accessed in web browser by visiting the domain name or public IP address configured, followed by */todo_list.php*:

```
http://<Public_domain_or_IP>/todo_list.php
```

![Project 2, 23](https://user-images.githubusercontent.com/68804488/150677687-22adc950-b81b-43d9-a59b-637e8851f6c5.png)


PHP environment is ready to connect and interact with the MySQL server.













