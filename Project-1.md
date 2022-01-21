# **Project 1** - LAMP STACK IMPLEMENTATION
## **Step 1 - Installing Apache and Updating the Firewall**
The below cmdlet updates a list of packages in package manager

```$ sudo apt update```

![screenshot1](https://user-images.githubusercontent.com/68804488/149378726-89336cae-9e94-49f7-9e48-01e16d56be16.png)

The below cmdlet runs apache2 package installation

```$ sudo apt install apache2```

![Screenshot 2a](https://user-images.githubusercontent.com/68804488/149379652-21cf225a-acef-4c60-bfdf-c6bef728bc56.png)

![Screenshot 2b](https://user-images.githubusercontent.com/68804488/149379757-acc9c0dc-e2a8-4db5-a9fb-f0811d3a84ce.png)

This code verifies that apache2 is running as a Service in my Server

```$ sudo systemctl status apache2```

![Screenshot 3](https://user-images.githubusercontent.com/68804488/149380541-aede62d7-c721-40cb-b361-571aef37542a.png)


Opened TCP port 80 in the EC2 Instance which is the default port that web browsers use to access web pages on the Internet

![Screenshot 4](https://user-images.githubusercontent.com/68804488/149380714-ca2117af-6d64-4f57-88ee-d8b9f9838f3e.png)

Output to check if we can access the apache2 locally

![Screenshot 5](https://user-images.githubusercontent.com/68804488/149381032-4cdd9ab1-7d0a-4ff5-bb37-26463271cc70.png)

Output to check if we can access the apache2 over the internet from any IP address

![Screenshot 9](https://user-images.githubusercontent.com/68804488/149382697-9572eb08-ab1b-4df0-a696-054bb5d984ea.png)

Test that apache server is running over the browser

![Screenshot 7](https://user-images.githubusercontent.com/68804488/149383062-4b9bfb69-b12e-45b4-8cfb-cf9b143add9b.png)


**Step 2** - Installing MySQL 

The below cmdlet installs the mysql in the server

```$ sudo apt install mysql-server```

![Screenshot 8](https://user-images.githubusercontent.com/68804488/149383784-d0076d79-e2a3-4d06-860d-2eaa68a00f31.png)

The below cmdlet runs a security script that removes sime insecure default settings and lock down access to the database system

```$ sudo mysql_secure_installation```

![Screenshot 10](https://user-images.githubusercontent.com/68804488/149384439-97365f1c-062e-4d37-8116-e65146a25ccc.png)

This code verifies that we can log in to the MySQL server

```$ sudo mysql```
 
![Screenshot 11](https://user-images.githubusercontent.com/68804488/149387747-a2412976-49df-462d-a7de-66e7ae692a65.png)

**Step 3** Installing PHP

The below cmdlet installs *php*, *libapache2-mod-php* and *php-mysql*

```$ sudo apt install php libapache2-mod-php php-mysql```

![Screenshot 12](https://user-images.githubusercontent.com/68804488/149402682-0d3b96be-46aa-445b-8a20-7d39a3b3f29d.png)


Use the below to confirm your php version

```$ php -v```

![Screenshot 13](https://user-images.githubusercontent.com/68804488/149402882-ee3d093b-fcb6-4658-9e7d-65f3a9189807.png)

**LAMP stack has been installed completely and it is operational**

**Step 4** - Creating a Virtual Host for your Website using Apache 2

Created a directory for projectlamp using ‘mkdir’ command

```$ sudo mkdir /var/www/projectlamp```

Assigned ownership to the directory with this variable $USER which still referenced the system user

```$ sudo chown -R $USER:$USER /var/www/projectlamp```

Created and opened a new configuration file in Apache’s sites-available directory using "vi" command-line editor

```$ sudo vi /etc/apache2/sites-available/projectlamp.conf```

Pasted the below bare-bones configuration by hitting on i on the keyboard to enter the insert mode, and pasted the text:

```
<VirtualHost *:80>
    ServerName projectlamp
    ServerAlias www.projectlamp 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/projectlamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Used the below cmdlet to show the new file we created in the sites-available directory

```$ sudo ls /etc/apache2/sites-available```

See screenshot of the above cmds ran

![Screenshot 14](https://user-images.githubusercontent.com/68804488/149506264-1f0750cf-59ab-48b4-8dd1-e5c1c91e7b02.png)

![Screenshot 16](https://user-images.githubusercontent.com/68804488/149506670-00fa127e-eafc-4e52-b31c-45a2f11ed1a3.png)


Created an index.html file in the /var/www/projectlamp location for testing if the virtual host works fine

```sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html```

The below screen shot is the output which shows Virtual host is working properly

![Screenshot 15](https://user-images.githubusercontent.com/68804488/149508052-77750a2d-5b9e-4f2f-8325-920a731dc884.png)

**Step 5** - Enable PHP on the website

We needed to edit the /etc/apache2/mods-enabled/dir.conf file and change the order in which the index.php file is listed within the DirectoryIndex directive so as to allow the php page be the landing page

```sudo vim /etc/apache2/mods-enabled/dir.conf```

```
<IfModule mod_dir.c>
        #Change this:
        #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
        #To this:
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
```

![Screenshot 17](https://user-images.githubusercontent.com/68804488/149508725-4beec0fa-9bc2-4425-ae0b-16599a982156.png)

PHP installation is working as expected

![Screenshot 18](https://user-images.githubusercontent.com/68804488/149509314-8c613b95-166f-483b-8cf9-5aa223c0d6ff.png)



