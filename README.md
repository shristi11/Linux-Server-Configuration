# Linux-Server-Configuration

## Server Details
IP address: `165.227.83.46`

SSH port: `2200`

URL: http://165.227.83.46/

## Secure your server

Update all currently installed packages.

`apt-get update` 

`apt-get upgrade` 

Change the SSH port from 22 to 2200

`sudo nano /etc/ssh/sshd_config` Change Port 22 to 2200

`sudo service ssh restart`

Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123).

`sudo ufw allow 2200/tcp`

`sudo ufw allow 80/tcp`

`sudo ufw allow ntp`

`sudo ufw enable`

## Give grader access

Create a new user account named grader

`adduser grader`

Give grader the permission to sudo

`gpasswd -a grader sudo`

Create an SSH key pair for grader using the ssh-keygen tool

`ssh-keygen`

`cat .ssh/id_rsa.pub` then copy the ssh key

## Prepare to deploy your project

Configure the local timezone to UTC.

`sudo dpkg-reconfigure tzdata`

Install and configure Apache to serve a Python mod_wsgi application

`sudo apt-get install apache2`

`sudo apt-get install libapache2-mod-wsgi python-dev`

`sudo service apache2 start`

 Install and configure PostgreSQL   ` `
 
 `sudo apt-get install libpq-dev python-dev`
 
 `sudo apt-get install postgresql postgresql-contrib`
 
 `sudo su - postgres`
 
 `psql`
 
`# CREATE USER catalog WITH PASSWORD 'password';`

`# ALTER USER catalog CREATEDB;`

`# CREATE DATABASE catalog WITH OWNER catalog;`

`# \c catalog`

`# REVOKE ALL ON SCHEMA public FROM public; `

`# GRANT ALL ON SCHEMA public TO catalog;`

`# \q`

`exit`

Install git 

`sudo apt-get install git`

Deploy the Item Catalog project.

`cd /var/www`

`sudo mkdir FlaskApp`

`cd /FlaskApp`

`git clone `

` `

` `

` `

` `

` `

` `

` `

` `

` `
