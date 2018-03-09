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

# Deploy the Item Catalog project.

`cd /var/www`

`sudo mkdir FlaskApp`

`cd /FlaskApp`

`git clone https://github.com/shristi11/Catalog-Application FlaskApp `

`sudo nano flaskapp.wsgi` Insert the following pythone code.

```python
#!/usr/bin/python
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0, "/var/www/FlaskApp/")

from FlaskApp import app as application
```
Install virtual environment, Flask and the project's dependencies

`sudo apt-get install python-pip`

`sudo pip install virtualenv`

`sudo virtualenv venv`

`source venv/bin/activate`

`sudo chmod -R 777 venv`

`pip install Flask`

`pip install httplib2 request oauth2client sqlalchemy python-psycopg2`

`sudo nano /etc/apache2/sites-available/FlaskApp.conf`

Insert the following cade:

```
<VirtualHost *:80>
    ServerName 165.227.83.46
    ServerAlias http://165.227.83.46/
    ServerAdmin root@ubuntu1
    WSGIDaemonProcess catalog python-path=/var/www/FlaskApp:/var/www/FlaskApp/FlaskApp/venv/lib/python2.7/site-packages
    WSGIProcessGroup FlaskApp
    WSGIScriptAlias / /var/www/FlaskApp/flaskapp.wsgi
    <Directory /var/www/FlaskApp/FlaskApp/>
        Order allow,deny
        Allow from all
    </Directory>
    Alias /static /var/www/FlaskApp/FlaskApp/static
    <Directory /var/www/FlaskApp/FlaskApp/static/>
        Order allow,deny
        Allow from all
    </Directory>
    ErrorLog ${APACHE_LOG_DIR}/error.log
    LogLevel warn
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
