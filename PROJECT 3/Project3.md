## Documentation for LAMP STACK IMPLEMENTATION

# The Project shows how to Implement LAMP(Linux,Apache,Mysql,Php) on AWS

After creating a new Ubuntu EC2 instance(an instance of a virtual server) on AWS, connect to the instance using `ssh`

`ssh -i <private-key-name>.pem ubuntu@<Public-IP-address>`

![Alt text](<images/ssh -i.png>)

## Installing Apache and Updating the Firewall

After launching the Ubuntu Virtual machine on AWS update it:

`sudo apt update`

Next, install apache2

`sudo apt install apache2`

![Alt text](<images/sudo apt install apache2.png>)

To verify that apache2 is running as a Service in the ubuntu instance, enter the command:

`sudo systemctl status apache2`

![Alt text](<images/check apche2 status.png>)

Verify apache2 is accessible locally on the Ubuntu shell:

`curl http://localhost:80 or curl http://127.0.0.1:80`

![Alt text](<images/accessing apache on ubuntu.png>)

Verify apache is accessible from the internet:

`http://<Public-IP-Address>:80`

![Alt text](<images/accessing apache on browser.png>)

## Installing Mysql

Install my sql on ubuntu server:

`sudo apt install mysql-server`

![Alt text](<images/installing mysql-server.png>)

Log into the MySQL console,

`sudo mysql`

Next, configure a database user on and setting login password for Mysql

`ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';`

![Alt text](<images/login to mysql console.png>)

Exit the shell with `exit`

Start MYSQL Interactive script, prompting you to configure the validate password plugin

`sudo mysql_secure_installation`

![Alt text](<images/setting mysql through interactive script 1.png>)
![Alt text](<images/setting up mysql through interactive script 2.png>)

Confirm ability to login to Mysql running the command:

`sudo mysql -p`

![Alt text](<images/testing to log into mysql console.png>)

To exit the MySQL console, type:

`mysql> exit`

## Installing PHP

3 packages will be installed namely php, libapache2-mod-php, php-mysql, by running this command to install all packages listed.

`sudo apt install php libapache2-mod-php php-mysql`

![Alt text](<images/installing php libapache2-mode-php & php-mysql.png>)

Confirm the php version

`php -v`

![Alt text](<images/php version.png>)

At this point, all 4 applications that make up the lamp stack have been successfully installed;

- Linux
- Apache Http Server
- MySQL
- PHP

## Creating a Virtual Host for a Website using Apache

Create directory called projectlamp

`sudo mkdir /var/www/projectlamp`

Next, assign ownership of the directory with current system user

`sudo chown -R $USER:$USER /var/www/projectlamp`

![Alt text](<images/mkdir for projectlamp and assign ownership.jpg>)

Create and open a new configuration file in Apache’s sites-available directory.

`sudo vi /etc/apache2/sites-available/projectlamp.conf`

![Alt text](<images/opening projectlamp.conf.jpg>)

Paste in the following bare-bones configuration

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

![Alt text](<images/bare-bones config.png>)

list the new file in the sites-available directory.

`sudo ls /etc/apache2/sites-available`

![Alt text](<images/list sites-available directory.png>)

Thus, we have set /var/www/projectlampl as the root directory for Projectlamp.

Enable the new virtual host with the a2ensite command:

`sudo a2ensite projectlamp`

![Alt text](<images/enable virtual host.png>)

Disable the default site with a2dissite command:

`sudo a2dissite 000-default`

![Alt text](<images/disable default site with a2dissite command.png>)

Run the below command to check for syntax errors in the configuration file.

`sudo apache2ctl configtest`

![Alt text](<images/syntax error check.png>)

Reload Apache service for changes to take efffect

`sudo systemctl reload apache2`

Create an index file in the projectlamp folder.

![Alt text](<images/create an index file in projectlamp.png>)

Insert this code in the index.html file

```
sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html
```

![Alt text](<images/insert in index file.png>)

Open on browser and open the website URL using the IP address:

`http://<Public-IP-Address>:80`

![Alt text](<images/result of index file on browser.png>)

## Enable PHP on the website

With the default DirectoryIndex settings on Apache, the index.html file takes precedence, it needs to be modified and give precedence to the index.php file.
The /etc/apache2/mods-enabled/dir.conf file needs to be edited and changed the order in which the index.php file is listed within the DirectoryIndex directive:

`sudo vim /etc/apache2/mods-enabled/dir.conf`

```
<IfModule mod_dir.c>
        #Change this:
        #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
        #To this:
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
```

![Alt text](<images/changing the dir.conf file.png>)

Save and close the file, The Apache service needs to be restarted for the changes to take effect.

`sudo systemctl reload apache2`

Create a new file named index.php inside the projectlamp root folder:

`vim /var/www/projectlamp/index.php`

![Alt text](<images/creating a new file in the custom web root folder(index.php).png>)

Paste in this PHP code inside the index.php file:

```
<?php
phpinfo();
```

![Alt text](<images/adding valid php code into index.php file.png>)

Open on browser with the IP address/index.php:

`http://<Public-IP-Address>:80/index.php`

![Alt text](<images/php on browser.png>)

After checking the relevant information, it is best to remove the file as it contains sensitive information about my PHP enviroment and my Ubuntu Server

`sudo rm /var/www/projectlamp/index.php`

![Alt text](<images/remove index.php file.png>)
