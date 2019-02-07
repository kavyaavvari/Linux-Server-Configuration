# Linux Server Configuration
> by Kavya Avvari

This is the third project for the Udacity Fullstack Nanodegree. This project involves securing and setting up a Linux server. To complete this project, a Linux Server instance was setup on Amazon Lightsail. 

## Server Info

* **Public IP Address:** 18.222.106.206

* **SSH Port:** 2200

## Get your server

Start a new Ubuntu Linux server instance on Amazon Lightsail.

1. Login
2. Create an instance. 
3. Choose an instance image.
4. Choose an instance plan: OS Only (Ubuntu).
5. Give your instance a unique name.
6. Create instance!
7. Once the instance has started up, connect to it using SSH.

## Secure your server

1. Update all currently installed packages. 
```
sudo apt-get update
sudo apt-get upgrade
```

2. Change the SSH port from 22 to 2200. Make sure to configure the Lightsail firewall to allow it.
```
sudo vi /etc/ssh/sshd_config
```
* After opening the file, change the port from 22 to 2200. 

3. Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123).
```
sudo ufw status                 
sudo ufw default deny incoming  
sudo ufw default allow outgoing 
sudo ufw allow 2200/tcp         
sudo ufw allow 80/tcp
sudo ufw allow 123/tcp        
sudo ufw enable                 
sudo ufw status     
```
> Warning: When changing the SSH port, make sure that the firewall is open for port 2200 first, so that you don't lock yourself out of the server. When you change the SSH port, the Lightsail instance will no longer be accessible through the web app 'Connect using SSH' button. The button assumes the default port is being used. There are instructions on the same page for connecting from your terminal to the instance. Connect using those instructions and then follow the rest of the steps.

## Give grader access
In order for your project to be reviewed, the grader needs to be able to log in to your server.

1. Create a new user account named grader.
```
sudo adduser grader
```
2. Give grader the permission to sudo.
```
sudo vi /etc/sudoers.d/grader
```
And add this line to the file:
```
grader ALL=(ALL) NOPASSWD:ALL
```

3. Create an SSH key pair for grader using the ssh-keygen tool by running the command: 
```
ssh-keygen -t rsa
```
* Save the private key on your local machine
* Save the public key on the server in a file called ```authorized_keys```. This file should be located in a ```.ssh``` directory so make a directory with that name so the file can be stored. 

To login to grader, run the command:
```
ssh grader@18.222.106.206 -p 2200 -i ~/.ssh/pgrader
```

## Prepare to deploy your project
1. Configure the local timezone to UTC.
```
sudo dpkg-reconfigure tzdata
```

2.  Install and configure Apache to serve a Python mod_wsgi application.
```
sudo apt-get install apache2
```
* Install library functions of apache using the command:
```
sudo apt-get install libapache2-mod-wsgi-py3
```
* Configure Apache to handle requests using the WSGI module by opening the 000-default.conf file.
```
sudo vi /etc/apache2/sites-enabled/000-default.conf
```
* Add the following line ```WSGIScriptAlias / /var/www/html/myapp.wsgi``` right before the closing of ```</VirtualHost>```
* Enable the mod_wsgi using the command:
```
sudo a2enmod wsgi
```
* Install some libraries of python development:
```
sudo apt-get install libpq-dev python-dev
```
3. Install and configure PostgreSQL:
```
sudo apt-get install postgresql
```
* Do not allow remote connections.
* Create a new database user named catalog that has limited permissions to your catalog application database.
```

```

