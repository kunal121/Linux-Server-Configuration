# Linux-Server-Configuration
### Introduction
> Setting up a linux server to host our web application,secure it from various attacks complete description of how to setup,configure and deploy our server
### Live version of project
* [Item-catalog](http://ec2-13-126-69-17.ap-south-1.compute.amazonaws.com/)
* public ip address :- 13.126.69.17
## How to configure the linux server
### 1. Setting up development enviroment
* click [here](https://lightsail.aws.amazon.com) to setup a development enviroment.
* create a instance and then you will get a public ip address.
* Download the private key.
### 2. What to do with private key?
* Move the private key in a folder on your machine
* change the access of the private key so only owner can access .
``` 
$ chmod 600 ~/.ssh/key 
```
* ssh into the instance .
 ```
 $ ssh -i ~/.ssh/key.rsa root@public_IP_address
 ```
 ### 3. Create a new user
 * command to add user.
 ```
 sudo adduser grader 
 ```
 * try to run .
 ```
 sudo/etc/var
 ```
 * it shows the error because grader has no access to sudo .
 * To give grader access to sudo .
 ```
 $ sudo nano /etc/sudoers.d/grader
 ```
 * add the text to the craeted file
 ```
 grader ALL=(ALL:ALL) ALL
 ```
 * Now again run the command 
 ```
 sudo/etc/var
 ```
 * now the above command will work because now grader have access to sudo
 #### setting up ssh for user
 - login as user
 - touch .ssh/authorized_keys
 - chmod 700 .ssh
 ### 4.UFW COMMANDS(Uncomplicated firewall)
 ```
 $ sudo ufw default deny incoming
 $ sudo ufw default allow outgoing
 $ sudo ufw allow 2200/tcp
 $ sudo ufw allow www
 $ sudo ufw allow ntp
 $ sudo ufw enable
 ```
 ### 5. Configure cron scripts to automatically manage package updates
 ```
 $ sudo apt-get install unattended-upgrades
 $ sudo dpkg-reconfigure --priority=low unattended-upgrades
 $ sudo apt-get install apache2 libapache2-mod-wsgi git
 $ sudo a2enmod wsgi 
 ```
 ### 6.Install Postgresql
 ```
 $ sudo apt-get install libpq-dev python-dev
 $ sudo apt-get install postgresql postgresql-contrib
 $ sudo su - postgres
 $ psql
 ```
 ### 7.Configure Postgresql
 ```
 * Create user kunal with password 'xyz'
 * Create database food with owner kunal
 * \c catalog
 * REVOKE ALL ON SCHEMA public FROM public;
 * GRANT ALL ON SCHEMA public TO catalog
 * \q
 ```
 ### 8.Changing the path inside flask application
 ```
 engine = create_engine('postgresql://kunal:yourPassword@localhost/food')
 ```
 ### 9. Install Flask and SqlAlchemy
 ```
    $ sudo apt-get install python-pip
    $ sudo pip install Flask
    $ sudo pip install httplib2 oauth2client sqlalchemy psycopg2 sqlalchemy_utils
    $ sudo pip install requests
 ```
 ### 10. Clonning the repository
 ```
 * clone the repository from [here](https://github.com/kunal121/Item-catalog)
 * switch to the branch deployment by command git checkout deployment
```
### 11.creating a wsgi file with given content
```
$ touch bookCatalog.wsgi && nano bookCatalog.wsgi
```
```
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0, "/var/www/Item-catalog/")
from project import app as application
```
### 12. craeting dummy database by executing file
```
 $ python database_setup.py
 $ python dummybooks.py
```
### 13. configure 000-default.conf with following content under apache
```
<VirtualHost *:80>
  ServerName XX.XX.XX.XX
  ServerAdmin kunalkalra121@gmail.com
  WSGIScriptAlias / /var/www/Item-catalog/Item-catalog.wsgi
  <Directory /var/www/Item-catalog/>
      Order allow,deny
      Allow from all
  </Directory>
  Alias /static /var/www/Item-catalog/static
  <Directory /var/www/Item-catalog/static/>
      Order allow,deny
      Allow from all
  </Directory>
</VirtualHost>
```
### 14. Run server again
```
 $ sudo service apache2 restart
```
 
 
