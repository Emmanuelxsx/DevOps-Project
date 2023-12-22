# AWS LEMP STACK IMPLEMENTATION

## This project shows how to implement LEMP(Linux,Nginx,Mysql,PHP) on AWS

Connect to AWS Ubuntu EC2 instance with `ssh`

`ssh -i <Your-private-key.pem> ubuntu@<EC2-Public-IP-address>`

![Alt text](<images/connecting to instance.png>)

### Installing Nginx Webserver

Update the list of package in the instance

`sudo apt update `

![Alt text](<images/update server with apt.png>)

Install Nginx with `apt install`

`sudo apt install nginx -y`

![Alt text](<images/installing nginx.png>)

Verify Nginx is up and running

`sudo systemctl status nginx`

![Alt text](<images/verify nginx installation.png>)

Open port 80 in the instance

![Alt text](<images/inbound connection on port 80 enabled.png>)

Verify the web server is reachable from the localhost

`curl http://localhost:80` or `curl http://127.0.0.1:80`

![Alt text](<images/accessing nginx locally on Ubuntu shell.png>)

Access Nginx from browser

`http://<Public-IP-Address>:80`

![Alt text](<images/accessing nginx on browser.png>)

### Installing MySQL

Install MYSQL package using apt command

`sudo apt install mysql-server`

![Alt text](<images/installing mysql.png>)

Log into Mysql console

`sudo mysql`

Next, set root password for Mysql

`ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';`

![Alt text](<images/login to mysql shell.png>)

Exit the MySQL shell with `exit`:

`mysql> exit`

![Alt text](<images/exit console 1.png>)

Run the security with the command

`sudo mysql_secure_installation`

![Alt text](<images/interative mysql shell.png>)

This will ask if you want to configure the VALIDATE PASSWORD PLUGIN, After the validation is completed, login to MYSQL and input the password by adding the -p flag.

`sudo mysql -p`

![Alt text](<images/login to mysql to retrieve data from mysql database.png>)

After confirmation of login is successful, you can exit MYSQL console

![Alt text](<images/exit console.png>)

### Installing PHP

Install php-fpm, which stands for “PHP fastCGI process manager”, and tell Nginx to pass PHP requests to this software for processing. Additionally, php-mysql is needed, a PHP module that allows PHP to communicate with MySQL-based databases. Core PHP packages will automatically be installed as dependencies

`sudo apt install php-fpm php-mysql`

![Alt text](<images/installing php.png>)

### Configuring Nginx to Use PHP Processor

Create the root web directory for ** my_domain ** as follows:

`sudo mkdir /var/www/projectLEMP`

![Alt text](<images/creating a root web directory.png>)

Next, assign ownership of the directoy with $USER environment variable, which will reference the current system user:

`sudo chown -R $USER:$USER /var/www/projectLEMP`

![Alt text](<images/assign ownership to currect system user.png>)

Open a new configuration file in Nginx's `sites-available` directory using nano command-line editor

`sudo nano /etc/nginx/sites-available/projectLEMP`

![Alt text](<images/open config file with nano.png>)

Copy and paste in the following bare-bones configuration:

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
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

}
```

![Alt text](<images/bare-bones configuration.png>)

Activate the configuration by linking to the config file from Nginx's `sites-enabled` directory:

`sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/`

![Alt text](<images/activate config file.png>)

Test the configuraton for syntax errors:

`sudo nginx -t`

![Alt text](<images/test config file for syntax error.png>)

Disable Nginx host that is currently configured to listen on port 80, for this run:

`sudo unlink /etc/nginx/sites-enabled/default`

![Alt text](<images/disabling default nginx host listening on port 80.png>)

Reload Nginx to apply the changes:

`sudo systemctl reload nginx`

![Alt text](<images/reload nginx to apply changes.png>)

Open the website URL from the browser using IP address

`http://<Public-IP-Address>:80`

![Alt text](<images/opening index.html website URL on browser.png>)

### Testing PHP with Nginx

LEMP stack is completely set up.
To test the validity that Nginx can correctly hand `.php` files off to PHP processor;
Create a test PHP file in the document root: open a new file called `info.php` within the document root

`nano /var/www/projectLEMP/info.php`

Copy and paste the valid PHP code that will return information about the server

```
<?php
phpinfo();
```

![Alt text](<images/valid PHP code.png>)

Open on browser the webpage containing detailed information about the server by visiting the public address set up in the Nginx config file followed by /info.php:

`http://`server_domain_or_IP`/info.php`

![Alt text](<images/opening info.php on browser.png>)

### Retrieving Data from Mysql Database with PHP

create a database named lemp_database and a user named lemp_user.

First, connect to the MySQL console using the root account and password:

`sudo mysql -p`

![Alt text](<images/login to mysql to retrieve data from mysql database.png>)

Create a new database and user

![Alt text](<images/creating example_database.png>)

Create a new user and grant full privileges on the database that have just been created

`mysql>  CREATE USER 'test_user'@'%' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';`

Next, give this user permission over the `example_database` database:

`mysql> GRANT ALL ON example_database.* TO 'example_user'@'%';`

![Alt text](<images/creating example_user and giving permission over example_database.png>)

EXit the Mysql console

`mysql> exit`

![Alt text](<images/exit console.png>)

Test the new user has the proper permissions by logging into the MySQL console again, using the custom user credentials

`mysql -u example_user -p`

![Alt text](<images/logining in to the new user(example_user).png>)

Confirm access to the `example_database`

`mysql> SHOW DATABASES;`

![Alt text](<images/show database of example_user.png>)

Next, Create a test table named todo_list with the following statement:

`mysql> CREATE TABLE example_database.todo_list (item_id INT AUTO_INCREMENT,content VARCHAR(255),PRIMARY KEY(item_id));`

```
CREATE TABLE example_database.todo_list (item_id INT AUTO_INCREMENT,content VARCHAR(255),PRIMARY KEY(item_id));
```

![Alt text](<images/creating a todo_list.png>)

Insert a few rows of content in the table:

`mysql> INSERT INTO example_database.todo_list (content) VALUES ("My first important item");`

`mysql> INSERT INTO example_database.todo_list (content) VALUES ("My second database item");`

`mysql> INSERT INTO example_database.todo_list (content) VALUES ("My third database item")`

`mysql> INSERT INTO example_database.todo_list (content) VALUES ("and this one more thing");`

![Alt text](<images/inserting few items in the todo_list.png>)
![Alt text](<images/inserting few items in the todo_list2.png>)

To confirm that the data was successfully saved to the table, run:

`mysql>  SELECT * FROM example_database.todo_list;`

![Alt text](<images/to confirm if data successfully saved.png>)

After confirming if the data in the test table is valid, exit the MySQL console:

`mysql> exit`

![Alt text](<images/exit console 1.png>)

Create a PHP script that will connect to MySQL and query the content. Create a new PHP file in the custom web root directory

`nano /var/www/projectLEMP/todo_list.php`

![Alt text](<images/creating a PHP file in custom web root dir.png>)

Copy this content into the `todo_list.php` script:

```
<?php
$user = "example_user";
$password = "PassWord.1";
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

![Alt text](<images/content in the todo_list.php.png>)

Access the todo_list.php page from web browser

![Alt text](<images/accessing todo_list.php on browser.png>)
