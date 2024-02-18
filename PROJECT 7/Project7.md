## Implementing Loadbalancers with Nginx

# Introduction to Load Balancing and Nginx




In computer terms, load balancing means distributing the work or tasks among several computers or servers so that no one computer gets overload with too much work.

![alt text](<images/load balancer representation.PNG>)

# Setting Up a Basic Load Balancer

**Step 1**: Provision 2 EC2 instance as the two webservers and a load balancer server.

**Step 2**: Open Port 8000 for the webservers while the load balancer will run on port 80.

![alt text](<images/enabling port 8000.PNG>)

**Step 3**: Connect webservers and Install Apache Webserver on both servers

![alt text](<images/connect to server.PNG>)

![alt text](<images/connect to server 2.PNG>)

Next, install apache with the command `sudo apt update -y &&  sudo apt install apache2 -y` 

![alt text](<images/install apache.PNG>)

Verify that apache is running using the command below:

`sudo systemctl status apache2`

![alt text](<images/checking the status of apache.PNG>)

**Step 4**: Configure Apache to serve a page showing its public IP:

1. First, we need to configure Apache webserver to serve content on port 8000 instead of its default which is port 80.
Configure Apache to serve content on port 8000 using the text editor(e.g vi,nano) open the file /etc/apache2/ports.conf
`sudo vi /etc/apache2/ports.conf`

2. Add a new Listen directive for port 8000

![alt text](<images/enabling port 8000 in port.conf file.PNG>)

3. Next open the file /etc/apache2/sites-available/000-default.conf and change port 80 on the virtualhost to 8000 like the screenshot below:

`sudo vi /etc/apache2/sites-available/000-default.conf`

![alt text](<images/enabling 8000 in 000-default.conf file.PNG>)

4. Restart apache to load the new configuration using the command below:

`sudo systemctl restart apache2`

![alt text](<images/restart apache.PNG>)

* Creating the new html file:

1. Open a new index.html file with the command below:

`sudo vi index.html`

2. Switch vi editor to insert mode with i and paste the html file below. Before pasting the html file, get the public IP of the EC2 instance from AWS Management Console and replace the placeholder text for IP address in the html file. 

```
        <!DOCTYPE html>
        <html>
        <head>
            <title>My EC2 Instance</title>
        </head>
        <body>
            <h1>Welcome to my EC2 instance</h1>
            <p>Public IP: YOUR_PUBLIC_IP</p>
        </body>
        </html>
```

![alt text](<images/index.html contents.PNG>)

3. Change file ownership of the index.html file with the command below:
`sudo chown www-data:www-data ./index.html`

![alt text](<images/changing the file ownership of index.html file.PNG>)

* Overiding the Default html file of Apache Webserver:

1. Replace the default html file with the new html file  using the command below:
`sudo cp -f ./index.html /var/www/html/index.html`

![alt text](<images/replacing the default html file with the new one.PNG>)

2. Restart the webserver to load the new configuration using the command below:
`sudo systemctl restart apache2`

![alt text](<images/restart ngnix.PNG>)

3. A page like this should appear on the browser:

![alt text](<images/accessing the html file on browser on port 8000.PNG>)

**Step 5:** Configuring Nginx as a Load Balancer

* Provision a new EC2 instance running Ubuntu 22.0.4.Make sure port 80 is opened to accept traffic from anyhwere. 
* Next SSH into the instance.
* Install Nginx into the instance using the command below:

`sudo apt update -y && sudo apt install nginx -y`

![alt text](<images/installing ngnix.PNG>)

* Verify that Nginx is installed with the command below:
`sudo systemctl status nginx`

![alt text](<../PROJECT 4/images/verify nginx installation.png>)

* Open Nginx configuration file with the command below:

`sudo vi /etc/nginx/conf.d/loadbalancer.conf`

* Paste configuration file below to configure nginx to act like a load balancer. An example config file is shown below: Edit the file and provide necessary information like the server IP address etc.


````
        
        upstream backend_servers {

            # your are to replace the public IP and Port to that of your webservers
            server 127.0.0.1:8000; # public IP and port for webserser 1
            server 127.0.0.1:8000; # public IP and port for webserver 2

        }

        server {
            listen 80;
            server_name <your load balancer's public IP addres>; # provide your load balancers public IP address

            location / {
                proxy_pass http://backend_servers;
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            }
        }
    

````

![alt text](<images/configure to make ngnix act like a load balancer.PNG>)


**upstream backend_servers** defines a group of backend servers. The **server** lines inside the **upstream** block list the addresses and ports of your backend servers. **proxy_pass** inside the **location** block sets up the load balancing, passing the requests to the backend servers. The **proxy_set_header** lines pass necessary headers to the backend servers to correctly handle the requests

* Test the configuration with the command below:
`sudo nginx -t`

![alt text](<images/testing configuration file.PNG>)

* If there are no errors, restart Nginx to load the new configiuration with the command below:
`sudo systemctl restart nginx`

![alt text](<images/restart ngnix.PNG>)

* Paste the public address of Nginx load balancer on the browser, you should see the same webpages served by the webservers

![alt text](<images/accessing webserver1 with ngnix load balancer IP addr.PNG>)

![alt text](<images/accessing webserver2 with ngnix load balancer IP addr.PNG>)