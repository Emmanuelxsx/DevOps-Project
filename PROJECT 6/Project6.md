## Understanding Client server architecture with MySQL as RDBMS

# Client-Server Architecture with MySQL

Client-Server refers to an architecture in which two or more computers are connected together over a network to send and receive requests between one another.

In their communication, each machine has its own role: the machine sending requests is usually referred to as "Client" and the machine responding(server) is called "Server".

### A simple diagram of Web Client-Server architecture with DB server is presented below:

![Alt text](<images/client server archiecture with DB server.PNG>)

A quick example of a Client-Server communication in action below;

 On Ubuntu or Windows terminal run `curl` command: `curl -Iv www.propitixhomes.com` 

The response from the remote server is shown below;

![Alt text](<images/curl command.PNG>)

# Implement a Client Server Architecture using MySQL Database Management System (DBMS).

1. Create and configure two Linux-based virtual servers(EC2 instances in AWS)

Server A name - `mysql server`
Server B name - `mysql client`

![Alt text](<images/mysql server and mysql client.PNG>)

2. On `mysql server` Linux server install MySQL server software with the command `sudo apt install mysql-server`.

![Alt text](<../PROJECT 4/images/installing mysql.png>)

3. On `mysql client` Linux server install MySQL client software.

4. By default, both the EC2 virtual servers are located in the same local virtal network, so they can communication with each other using local IP addresses.
MySQL server uses TCP port 3306 by dafault, so open the port by creating entry in 'Inbound rules' in 'mysql server' Security Group.

![Alt text](<images/opening port 3306.PNG>)

5. Configure MySQL server to allow connections from remote hosts.

`sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf `

Replace '127.0.0.1' to '0.0.0.0'

![Alt text](<images/replacing to 0.0.0.0.PNG>)

6. From `mysql client` Linux server connect remotely to `mysql server` Database Engine without using SSH. `mysql` utility must be used to perform this action.

(i) On `mysql server` Linux server create a Database `learningProject6` with the command `CREATE DATABASE`

`CREATE DATABASE learningProject6;`

![Alt text](<images/Creating a datase on mysql server.PNG>)

(ii) Create a new user `leke` and grant permission to the newly created database

![Alt text](<images/creating a database user and granting premission.PNG>)

Login with the new user `mysql -u leke -p`

![Alt text](<images/testing new database user.PNG>)


(iii) Connect from `mysql client` to `mysql server`:

`mysql -u leke -p -h "ip of mysql server"`

Show database to confirm connection
`Show databases;`

![Alt text](<images/remote access to mysql server.PNG>)


