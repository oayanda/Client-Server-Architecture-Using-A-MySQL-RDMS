Spin up two(2) AWS ec2 Ubuntu instances to configure mysql server and mysql client.
![alt text](images/1.png)

Login into the mysql server instance and update all packages and install mysql-server
```bash
sudo apt update -y sudo apt install mysql-server
```
![alt text](images/2.png)

Login into the mysql client and update the packages and install mysql client.
```bash
sudo apt update sudo apt install mysql-client -y
```
![alt text](images/3.png)

Create inbound rule in the security group to open the default MySql port for the server and allow only the client server ip.
![alt text](images/4.png)
![alt text](images/5.png)

For ease of identification, set the hostnames for both instances. Exit and login back again to see the changes. 
```bash
sudo hostnamectl set-hostname mysql-server                  
```
![alt text](images/6.png)
```bash
sudo hostnamectl set-hostname mysql-client                  
```
![alt text](images/7.png)

Set the Password for the root user.
![alt text](images/8.png)

Delete unsecure default settings by using the mysl security script on the server.
```bash
mysql_secure_installation
```
![alt text](images/9.png)

Create a user on mysql server.
```bash
CREATE USER 'client_user'@'%' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
```
Create a database and grant the created user access.
```bash
CREATE DATABASE testdb;
```
```bash
GRANT ALL ON testdb.* TO 'client_user'@'%';
```
Reloads the grant tables in the mysql database enabling the changes to take effect without reloading or restarting mysql service.
```bash
FLUSH PRIVILEDES;
```
![alt text](images/10.png)

Change the bind address to 0.0.0.0 to allow connect from a remote host.
```bash
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```
![alt text](images/11.png)
Restart the mqsl service.
```bash
sudo systemctl restart mysql
```
![alt text](images/12.png)

Test Connection
From the client instance, connect to the mysql server instance.
```bash
sudo mysql -u clent_user -h 172.31.85.132 -p
```
![alt text](images/13.png)