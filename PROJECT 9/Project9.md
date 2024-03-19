# Implementing Wordpress Web Solution

Three-tier Architecture Generally, web, or mobile solutions are implemented based on what is called the Three-tier Architecture.

Three-tier Architecture is a client-server software architecture pattern that comprise of 3 separate layers.


**A 3 tier Stepup  required**

-  A Laptop or PC to serve as a client
-  An EC2 Linux Server as a webserver(This is where WordPress will be installed)
- An EC2 Linux server as a database(DB) server

Step 1 - Prepare a Webserver

1. Create an EC2 AWS instance using RedHat Distribution
The EC2 instance will serve as a Web Server, create 3 volumes in the same AZ as the Web Srver EC2, each of 1GB.

![alt text](<images/the volume.PNG>)

2. Attach all three volumes one by one to the Webserver EC2 instance

![alt text](<images/attach voume.png>)

![alt text](<images/attaching volme 2.png>)

3. Open up the Linux terminal to begin configuration

![alt text](<images/connecting to the webserver ec2 instance.PNG>)

4. Use `lsblk` command to inspect what block devices are attached to the server. Notice names of your newly created devices. All devices in Linux reside in /dev/ directory. Inspect it with `ls /dev/` and make sure you see all 3 newly created block devices there – their names will likely be `xvdf`, `xvdh`, `xvdg`.

![alt text](<images/checking what block devices are attached to the server.PNG>)

with the command `ls /dev/`
![alt text](<images/checking what block devices are attached to the server 2.PNG>)

5. Use df -h command to see all mounts and free space on the server


6. Use `gdisk` utility to create a single partition on each of the 3 disks

using the command `sudo gdisk /dev/xvdf`

![alt text](<images/creating a single partition for disk 1.PNG>)

![alt text](<images/creating a single partition for disk 2.PNG>)

![alt text](<images/creating a single partition for disk 3.PNG>)

7. Use `lsblk` utility to view the newly configured partition on each of the 3 disks.

![alt text](<images/view newly configured partition on each of the 3 disks.PNG>)

8. Install `lvm2` package using `sudo yum install lvm2`. Run `sudo lvmdiskscan` command to check for available partitions.

![alt text](<images/installing lvm.PNG>)

9. Use `pvcreate` utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM.

using the commands: 

`sudo pvcreate /dev/xvdf1`

`sudo pvcreate /dev/xvdg1`

`sudo pvcreate /dev/xvdh1`

![alt text](<images/mark each of the 3 disks as physical volumes(PVs) to be used by LVM.PNG>)

10. Verify that the Physical volume has been created sucessfullly by runnning `sudo pvs`

![alt text](<images/verifying if physical volume has been created successfully.PNG>)

11. Use `vgcreate` utility to add all 3 PVs to a volume group (VG). Name the VG webdata-vg
using the command: 

`sudo vgcreate webdata-vg /dev/xvdh1 /dev/xvdg1 /dev/xvdf1`

![alt text](<images/add all PVs to a volume group.PNG>)

12.  Verify that the VG has been created successfully by running `sudo vgs`

![alt text](<images/verifying that the VG has been successfully created.PNG>)

13. Use `lvcreate` utility to create 2 logical volumes. apps-lv (Use half of the PV size), and logs-lv Use the remaining space of the PV size. NOTE: apps-lv will be used to store data for the Website while, logs-lv will be used to store data for logs.

```
sudo lvcreate -n apps-lv -L 1.4G webdata-vg
sudo lvcreate -n logs-lv -L 1.4G webdata-vg
```

![alt text](<images/lvcreate to create 2 logical volumes.PNG>)

13. Verify that your Logical Volume has been created successfully by running `sudo lvs`

![alt text](<images/verifying that the Logical Volume has been created.PNG>)

14. Verify the entire setup using the command:

`sudo vgdisplay -v #view complete setup - VG, PV, and LV`

`sudo lsblk`

![alt text](<images/verifying the entire setup.PNG>)

15. Use `mkfs.ext4` to format the logical volumes with `ext4` filesystem

```
sudo mkfs -t ext4 /dev/webdata-vg/apps-lv
sudo mkfs -t ext4 /dev/webdata-vg/logs-lv
```

![alt text](<images/formatting the logical volumes with ext4 filesystem.PNG>)

16. Create /var/www/html directory to store website files

`sudo mkdir -p /var/www/html`

![alt text](<images/creating html directory.PNG>)

17. Create /home/recovery/logs to store backup of log data

`sudo mkdir -p /home/recovery/logs`

![alt text](<images/creating dir to store backup of log data on db server.PNG>)

18. Mount /var/www/html on apps-lv logical volume

`sudo mount /dev/webdata-vg/apps-lv /var/www/html/`

![alt text](<images/mount html directory on  apps-lv logical volume.PNG>)

19. Use `rsync` utility to backup all the files in the log directory /var/log into /home/recovery/logs (This is required before mounting the file system)

`sudo rsync -av /var/log/. /home/recovery/logs/`

![alt text](<images/use rsync utility to backup all the files.PNG>)


20. Mount /var/log on logs-lv logical volume. (Note that all the existing data on /var/log will be deleted. That is why step 15 above is very important)

`sudo mount /dev/webdata-vg/logs-lv /var/log`

![alt text](<images/mount log on logs-lv logical volume.PNG>)

21. Restore log files back into /var/log directory

`sudo rsync -av /home/recovery/logs/. /var/log`

![alt text](<images/restore log files back into varlog directory.PNG>)

22. Update `/etc/fstab` file so that the mount configuration will persist after restart of the server.

The UUID of the device will be used to update the `/etc/fstab file`;

`sudo blkid`

![alt text](<images/update fstab file so that the mount configuration will persist after server restart.PNG>)

`sudo vi /etc/fstab`

Update `/etc/fstab` in this format using your own UUID and rememeber to remove the leading and ending quotes.

![alt text](<images/updating fstab file using UUID.PNG>)

23. Test the configuration and reload the daemon

```
sudo mount -a
sudo systemctl daemon-reload
```

![alt text](<images/testing the configurating and reloading daemon on db server.PNG>)

24. Verify your setup by running `df -h`, output must look like this:

![alt text](<images/verify the setup by running df -h.PNG>)

# Installing wordpress and configuring to use MySQL Database

Step 2 - Prepare the Database Server

Launch a second RedHat EC2 instance that will have a role – ‘DB Server’ Repeat the same steps as for the Web Server, but instead of `apps-lv` create `db-lv` and mount it to /db directory instead of `/var/www/html/`.

Step 3 - Install WordPress on your Web Server EC2

1. Update the repository

`sudo yum -y update`

![alt text](<images/updating the repo.PNG>)

2. Install wget, Apache and it's dependencies

`sudo yum -y install wget httpd php php-mysqlnd php-fpm php-json`

![alt text](<images/installing apache and its dependencies.PNG>)

3. Start Apache

````
sudo systemctl enable httpd
sudo systemctl start httpd

````

![alt text](<images/starting apache.PNG>)

4. To install PHP and its dependencies

`````
sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
sudo yum install yum-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm
sudo yum module list php
sudo yum module reset php
sudo yum module enable php:remi-7.4
sudo yum install php php-opcache php-gd php-curl php-mysqlnd
sudo systemctl start php-fpm
sudo systemctl enable php-fpm
setsebool -P httpd_execmem 1

`````
![alt text](<images/installing PHP and its dependencies.PNG>)

5. Restart Apache

`sudo systemctl restart httpd`

![alt text](<images/restarting httpd.PNG>)

6. Download wordpress and copy wordpress to var/www/html


````
mkdir wordpress
cd   wordpress
sudo wget http://wordpress.org/latest.tar.gz
sudo tar xzvf latest.tar.gz
sudo rm -rf latest.tar.gz
cp wordpress/wp-config-sample.php wordpress/wp-config.php
cp -R wordpress /var/www/html/

````

![alt text](<images/downloading wordpress and copy to html dir.PNG>)
![alt text](<images/downloading wordpress and copy to html dir 2.PNG>)

7. Configure SELinux Policies

````
 sudo chown -R apache:apache /var/www/html/wordpress
 sudo chcon -t httpd_sys_rw_content_t /var/www/html/wordpress -R
 sudo setsebool -P httpd_can_network_connect=1

````

![alt text](<images/configuring SELinux Policies.PNG>)

Step 4 - Install MySQL on the DB Server EC2

````
sudo yum update
sudo yum install mysql-server

````

![alt text](<images/installing MySQL on db server.PNG>)

Verify that the service is up and running by using sudo systemctl status mysqld, if it is not running, restart the service and enable it so it will be running even after reboot:

````
sudo systemctl restart mysqld
sudo systemctl enable mysqld

`````

![alt text](<images/starting mysql service.PNG>)

Step 5 - Configure DB to work with WordPress

```
sudo mysql

CREATE DATABASE wordpress;

CREATE USER `sql_user`@`<Web-Server-Private-IP-Address>` IDENTIFIED BY 'mypass';

GRANT ALL ON wordpress.* TO 'myuser'@'<Web-Server-Private-IP-Address>';

FLUSH PRIVILEGES;

SHOW DATABASES;

exit
````

![alt text](<images/configuring DB to work with wordpress1.PNG>)
![alt text](<images/configuring DB to work with wordpress2.PNG>)

Step 6 - Configure WordPress to connect to remote database.

Hint: Do not forget to open MySQL port 3306 on DB Server EC2. For extra security, you shall allow access to the DB server ONLY from your Web Server’s IP address, so in the Inbound Rule configuration specify source as /32

![alt text](<images/opening port 3306.PNG>)

1. Install MySQL client and test that you can connect from your Web Server to your DB server by using `mysql-client`

````
sudo yum install mysql
sudo mysql -u myuser -p -h <DB-Server-Private-IP-address>

````

![alt text](<images/installing msql on webserver.PNG>)
![alt text](<images/connecting webserver to db server by using mysql-client.PNG>)

2. Verify if you can successfully execute SHOW DATABASES; command and see a list of existing databases.

![alt text](<images/connecting webserver to db server by using mysql-client.PNG>)

3. Change permissions and configuration so Apache could use WordPress
 
4. Open and fill the DB credentials on `wp-config.php` file to allow connection to the remote MySQL database

`sudo vi wp-config.php`
 
![alt text](<images/editing the wp-config.php file to access database.PNG>)

4. Enable TCP port 80 in Inbound Rules configuration for your Web Server EC2 (enable from everywhere 0.0.0.0/0 or from your workstation’s IP)

![alt text](<images/opening port 80.PNG>)

5. Try to access from your browser the link to your WordPress http://<Web-Server-Public-IP-Address>/wordpress/

![alt text](<images/accessing on  browser.PNG>)
