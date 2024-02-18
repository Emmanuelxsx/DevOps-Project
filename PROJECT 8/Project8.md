## Automating Loadbalancer Configuration with Shell Scripting

# Automate the Deployment of Webservers

## Deploying and Configuring the Webservers

All the process needed to deploy the webservers has  been codified in the shell script below:

````
#!/bin/bash

####################################################################################################################
##### This automates the installation and configuring of apache webserver to listen on port 8000
##### Usage: Call the script and pass in the Public_IP of your EC2 instance as the first argument as shown below:
######## ./install_configure_apache.sh 127.0.0.1
####################################################################################################################

set -x # debug mode
set -e # exit the script if there is an error
set -o pipefail # exit the script when there is a pipe failure

PUBLIC_IP=$1

[ -z "${PUBLIC_IP}" ] && echo "Please pass the public IP of your EC2 instance as an argument to the script" && exit 1

sudo apt update -y &&  sudo apt install apache2 -y

sudo systemctl status apache2

if [[ $? -eq 0 ]]; then
    sudo chmod 777 /etc/apache2/ports.conf
    echo "Listen 8000" >> /etc/apache2/ports.conf
    sudo chmod 777 -R /etc/apache2/

    sudo sed -i 's/<VirtualHost \*:80>/<VirtualHost *:8000>/' /etc/apache2/sites-available/000-default.conf

fi
sudo chmod 777 -R /var/www/
echo "<!DOCTYPE html>
        <html>
        <head>
            <title>My EC2 Instance</title>
        </head>
        <body>
            <h1>Welcome to my EC2 instance</h1>
            <p>Public IP: "${PUBLIC_IP}"</p>
        </body>
        </html>" > /var/www/html/index.html

sudo systemctl restart apache2

````

Shell script for webserver1

![alt text](<images/install.sh content for server1.PNG>)

Shell script for webserver2

![alt text](<images/install.sh content for server2.PNG>)

## Follow the steps below to run the script:

**Step 1:** Provision 2 EC2 instance running on ubuntu 20.04. 

**Step 2:** Open port 8000 to allow traffic from anywhere using the security group.

![alt text](<../PROJECT 7/images/enabling port 8000.PNG>)

**Step 3:** Connect to the webservers via the terminal using SSH client

![alt text](<images/connection to server1.PNG>)

![alt text](<images/connection to server2.PNG>)

**Step 4:** Open a file, paste the script above and close the file using the command below:

`sudo vi install.sh`

![alt text](<images/install.sh content for server1.PNG>)

![alt text](<images/install.sh content for server2.PNG>)

To close the file type **esc** key then **Shift + :wqa!**

**Step 5:** Change the permissions of the file to make an executable using the command below:

`sudo chmod +x install.sh`

![alt text](<images/change permission.PNG>)

**Step 6:** Run the shell script using the command below.

`./install.sh PUBLIC_IP`

For webserver1
![alt text](<images/run install.sh script for server1.PNG>)
![alt text](<images/run install.sh script for server1(contd).PNG>)

For webserver2
![alt text](<images/run install.sh script for server2.PNG>)
![alt text](<images/run install.sh script for server2(contd).PNG>)

# Deployment of Nginx as a Load Balancer using Shell script

Deploy and configure Nginx Load Balancer using the shell script below:

````
#!/bin/bash

######################################################################################################################
##### This automates the configuration of Nginx to act as a load balancer
##### Usage: The script is called with 3 command line arguments. The public IP of the EC2 instance where Nginx is installed
##### the webserver urls for which the load balancer distributes traffic. An example of how to call the script is shown below:
##### ./configure_nginx_loadbalancer.sh PUBLIC_IP Webserver-1 Webserver-2
#####  ./configure_nginx_loadbalancer.sh 127.0.0.1 192.2.4.6:8000  192.32.5.8:8000
############################################################################################################# 

PUBLIC_IP=$1
firstWebserver=$2
secondWebserver=$3

[ -z "${PUBLIC_IP}" ] && echo "Please pass the Public IP of your EC2 instance as the argument to the script" && exit 1

[ -z "${firstWebserver}" ] && echo "Please pass the Public IP together with its port number in this format: 127.0.0.1:8000 as the second argument to the script" && exit 1

[ -z "${secondWebserver}" ] && echo "Please pass the Public IP together with its port number in this format: 127.0.0.1:8000 as the third argument to the script" && exit 1

set -x # debug mode
set -e # exit the script if there is an error
set -o pipefail # exit the script when there is a pipe failure


sudo apt update -y && sudo apt install nginx -y
sudo systemctl status nginx

if [[ $? -eq 0 ]]; then
    sudo touch /etc/nginx/conf.d/loadbalancer.conf

    sudo chmod 777 /etc/nginx/conf.d/loadbalancer.conf
    sudo chmod 777 -R /etc/nginx/

    
    echo " upstream backend_servers {

            # your are to replace the public IP and Port to that of your webservers
            server  "${firstWebserver}"; # public IP and port for webserser 1
            server "${secondWebserver}"; # public IP and port for webserver 2

            }

           server {
            listen 80;
            server_name "${PUBLIC_IP}";

            location / {
                proxy_pass http://backend_servers;   
            }
    } " > /etc/nginx/conf.d/loadbalancer.conf
fi

sudo nginx -t

sudo systemctl restart nginx

````

## Steps to Run the Shell Script

**Step 1:** On the terminal, open a file nginx.sh using the command below:

`sudo vi nginx.sh`

**Step 2:** Copy and paste the script inside the file

![alt text](<images/content of ngnix.sh.PNG>)

**Step 3:** Close the file using the command below:

`type esc the shift + :wqa!`

**Step 4:** Change the file permission to make it executable using the command below:

`sudo chmod +x nginx.sh`

![alt text](<images/change permission of ngnix.sh file.PNG>)

**Step 5:** Run the script with the command below:

`./nginx.sh PUBLIC_IP Webserver-1 Webserver-2`

![alt text](<images/run ngnix.sh.PNG>)
![alt text](<images/run ngnix.sh(contd).PNG>)

# Verifying the setup
## Screenshot for webserver1 on browser

![alt text](<images/access of webserver1 on browser.PNG>)

##  Screenshot for webserver2 on browser

![alt text](<images/access of webserver2 on browser.PNG>)

## Screenshot for load balancer displaying Webserver1 content

![alt text](<images/access of webserver1 through ngnix load balancer.PNG>)

## Screenshot for load balancer displaying Webserver2 content


![alt text](<images/access of webserver2 through ngnix load balancer.PNG>)

**Note:** Ip address changed due to a webserver restart

