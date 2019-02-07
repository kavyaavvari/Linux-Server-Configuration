# Linux Server Configuration
> by Kavya Avvari

This is the third project for the Udacity Fullstack Nanodegree. This project involves securing and setting up a Linux server. To complete this project, a Linux Server instance was setup on Amazon Lightsail. 

# Server Info

* **Public IP Address:** 18.222.106.206

* **SSH Port:** 2200

# Get your server

Start a new Ubuntu Linux server instance on Amazon Lightsail.

1. Login
2. Create an instance. 
3. Choose an instance image.
4. Choose an instance plan: OS Only (Ubuntu).
5. Give your instance a unique name.
6. Create instance!
7. Once the instance has started up, connect to it using SSH.

# Secure your server

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

# Give grader access
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

3. Create an SSH key pair for grader using the ssh-keygen tool.
