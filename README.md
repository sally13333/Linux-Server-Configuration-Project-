# Full Stack Web Developer Nanodegree Program
## Project :Linux Server Configuration Project
***
***

# Contents:

  - Linux Server Configuration Project
  - Requirements
  - IP & Hostname
  - software we installed
  - configuration changes
  - Application Deployment 
  - Authenticate login through google 
  -  Running the Item catalog application in server 
  - References

  
***
&nbsp;   
#### Linux Server Configuration Project:
>we will take a baseline installation of a Linux server and prepare it to host our web applications. we will secure our server from a number of attack vectors, install and configure a database server, and deploy one of our existing web applications onto it [Item catalog](https://github.com/sally13333/Item_Catalog_Project) ..

&nbsp;
***
#### Requirements:
 requirements would be:
  - [Python 3](https://www.python.org/downloads/)
  - [githup](https://github.com/)
  - Linux server instance [Amazon ightsail](https://lightsail.aws.amazon.com/)
  - git bash or any terminal 
  - There is some package we need it in linux server to run our application ,we will talk about it deeply in Application Deployment section   
 &nbsp; 
  `Note`:We're recommending Amazon Lightsail for this project. If you prefer, you can use any other service that gives you a publicly accessible Ubuntu Linux server. But Lightsail works pretty well and it's what we've tested.
***
#### IP & Hostname
-Host Name: ec2-3-16-47-246.us-east-2.compute.amazonaws.com.
-IP:3.16.47.246.
-Accessible SSH port: 2200.
-Application URL:http://ec2-3-16-47-246.us-east-2.compute.amazonaws.com
***
#### software we installed:
we recommend [Amazon Lightsail](https://lightsail.aws.amazon.com/)
- `Log in!`
First, log in to Lightsail. If you don't already have an Amazon Web Services account, you'll be prompted to create one by clicking` get started for free` . 
- `Create an instance`
Once you're logged in, Lightsail will give you a friendly message with a robot on it, prompting you to create an instance. A Lightsail instance is a Linux server running on a virtual machine inside an Amazon datacenter.
-`Choose an instance image: Ubuntu`
For this project, you'll want a plain Ubuntu Linux image. There are two settings to make here. First, choose "OS Only" (rather than "Apps + OS"). Second, choose `Ubuntu` as the operating system.
-`Choose your instance plan`
The instance plan controls how powerful of a server you get. It also controls how much money they want to charge you. For this project, the lowest tier of instance is just fine. And as long as you complete the project within a month and shut your instance down, the price will be zero.
- `Give your instance a hostname`
 You can give it any name you like but for us, we give it `udacity` 
- `Wait for it to start up`
It may take few
- you can find your puplic key under 'Public IP'
- `click "Account Page" at the bottom inside so you can download your private SSH key
- `click download`
to get your private key. The file type is .pem and will be used to SSH into the server so do not forget put it in `.ssh` directory 
- `Click the networking tab `
`Note:`please note that you inside the instance you have craeted before not in the main page ,this error took me two days to notice it 
- `click add another under "Firewall" and choose Custom for application, TCP for protocol, and the port number(123) under Port Range. Then click save`
-  `click add another under "Firewall" and choose Custom for application, TCP for protocol, and the port number(2200) under Port Range. Then click save`

##### 
***
#### configuration changes we made:
- open your `git bash `as administrator by right clicking on icon  
- change directory to `.shh`  by typing `cd .ssh` which we will store all of our SSH Keys 
`Note:`if you use Windows 10 ,you may not have this folder so you can create it by yourself in this directory /c/Users/[user name]
- To make our key secure type `$ chmod 600 ~/.ssh/udacity.pem` 
- to log into server type `$ ssh -i ~/.ssh/udacity.pem ubuntu@[public ID]`
-  switch to the root user by typing `sudo su -`
-  To create user called grader type` $ sudo adduser grader` ,it will ask 
    -  passwords type what ever you like but keep it in your mind  because it will ask you for it many times and do not forget what language are you type in 
    `Note:` when you type password you can not see anything 
    - few other fields which you can leave blank.
-  To create a file to give the user grader superuser privileges type `$ sudo nano /etc/sudoers.d/grader`
- The previous command will open [nano](https://en.wikipedia.org/wiki/GNU_nano) which is an editor in terminal ,then type `grader ALL=(ALL:ALL)ALL` then to save it `Ctrl+x` and `y`
-  To update installed package type `sudo apt-get update`
-  To upgrade installed package type `sudo apt-get upgrade`
-   To install new updates type `sudo apt-get dist-upgrade`
-   To see the users on this server we have install tool called Finger by typing`sudo apt-get install finger`
-To create an SSH Key for our new user grade type`ssh-keygen -f ~/.ssh/udacity.rsa`
- To read public key type `cat ~/.ssh/udacity.rsa.pub`,then copy it 
- Back in the server terminal locate the folder for the user grader, it should be `/home/grader`. Run the command ` cd /home/grader to move to the folder`
- To create a directory called .ssh type ` mkdir .ssh `
- To create a file to store the public key type `touch .ssh/authorized_keys`
- To edit that file type `nano .ssh/authorized_keys`and paste public key 
-  To change the permissions of the file type 
    -  `sudo chmod 700 /home/grader/.ssh`
    - `sudo chmod 644 /home/grader/.ssh/authorized_keys `
- To Change the owner of the .ssh directory from root to grade type `sudo chown -R grader:grader /home/grader/.ssh`
- To  restart  ssh service type  `sudo service ssh restart`
- To enforce key authentication type ` sudo nano /etc/ssh/sshd_config`.as we said this will open nano editor ,You may see many lines
    - Find the line that says  `Port 22` and change it to `Port 2200`  
    - Find the line that says`PermitRootLogin` and change it to `no`
    - Find the line that says `PasswordAuthentication` and change it to `no`
    - save it `Ctrl+x` and `y`
- type `sudo service ssh restart`to restart ssh again
- To configure the firewall type these commands:
    - `sudo ufw allow 2200/tcp`
    - `sudo ufw allow 80/tcp`
    - `sudo ufw allow 123/udp`
    - `sudo ufw enable`
- Here I recommend open new page in git bash as administrator  before you Disconnect from the server so that if you find any error you can fix it more easly so again in your new page type `cd .ssh `
-  to login with the grader account using ssh type `ssh -i ~/.ssh/udacity.rsa grader@[your public ID] -p 2200` you vave log in as `grader@ip-[your private IP]:~$`
- if you vave not face any error disconnect the First bage and keep the page we login with grader 
- To show all of the allowed ports with the firewall configuration type `sudo ufw status`
***
##
##### Application Deployment :
- If you close your terminal type `cd .ssh` and to login type  `ssh -i ~/.ssh/udacity.rsa grader@[your public ID] -p 2200`
- nstall required packages
   - `sudo apt-get install apache2`
   - ` sudo apt-get install libapache2-mod-wsgi python-dev`
   - `sudo apt-get install git`
- To Enable mod_wsgi type `sudo a2enmod wsgi`
- To  restart Apache `sudo service apache2 restart` 
`Note:`If you input the servers IP address into a web browser you'll see the Apache2 Ubuntu Default Page
- Set up the folder structure for our catalog application
    - `cd /var/www`
    - `sudo mkdir catalog`
    - `sudo chown -R grader:grader catalog`
    - `cd catalog `
- To clone our our Catalog Application repository catalog by type 
    - go to our Catalog Application repository
    - click on clone and copy URL
    - paste it in this command `git clone [repository url] catalog` 
- To make sure your secret key matches with your project secret key type ` sudo nano catalog.wsgi` after that paste the following 
 ``` import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0, "/var/www/catalog/")

from catalog import app as application
application.secret_key = 'super_secret_key'
```
- change name of your application file(what ever you called) to` __init__.py` by typing`mv application.py __init__.py`
- To create the .wsgi file (virtual environment directory ), make sure you are in `/var/www/catalog`.
   -`sudo pip install virtualenv`
   -` sudo virtualenv venv`
   - `source venv/bin/activate`
   -`sudo chmod -R 777 venv`
- If what you do is correct you should be in `(venv) grader@ip-[your private key ]:/var/www/catalog` ,in this directory you will install allpackages required for our Flask application like :
    - if you use python3 `sudo apt-get install python3-pip` if you use python 2 `sudo apt-get install python-pip` 
    - `sudo pip install flask`
    - `sudo pip install httplib2 oauth2client sqlalchemy psycopg2`
    - `sudo pip install requests`
    - `sudo pip install --upgrade oauth2client`
    - `sudo apt-get install libpq-dev`
    - `sudo pip install sqlalchemy_utils`
- open file `__init__.py` by changing directory to ` cd catalog`and then type
`sudo nano __init__.py` and change content for any line heve `client_secrets.json`or `fb_client_secrets.json` to `/var/www/catalog/catalog/client_secrets.json` then `Ctrl+x`and `y`
- To configure and enable our virtual host to run the site type `sudo nano /etc/apache2/sites-available/catalog.conf`and then paste :
``` 
<VirtualHost *:80>
    ServerName [Public IP]
    ServerAlias [Hostname]
    ServerAdmin admin@[Public IP] 
    WSGIDaemonProcess catalog python-path=/var/www/catalog:/var/www/catalog/venv/lib/python2.7/site-packages
    WSGIProcessGroup catalog
    WSGIScriptAlias / /var/www/catalog/catalog.wsgi
    <Directory /var/www/catalog/catalog/>
        Order allow,deny
        Allow from all
    </Directory>
    Alias /static /var/www/catalog/catalog/static
    <Directory /var/www/catalog/catalog/static/>
        Order allow,deny
        Allow from all
    </Directory>
    ErrorLog ${APACHE_LOG_DIR}/error.log
    LogLevel warn
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
``` 
`Note`you can find your hostname [here](https://whatismyipaddress.com/ip-hostname)
- To load  your site with the hostname.
  - Enable to virtual host by typing `sudo a2ensite catalog.conf`
  -  DISABLE the default host by typing `a2dissite 000-default.conf`
-To setting up the database 
   -`sudo apt-get install libpq-dev python-dev`
   -`sudo apt-get install postgresql postgresql-contrib`
   -`sudo su - postgres`
   -`psql`
- To create a database user and password
   -`postgres=# CREATE USER catalog WITH PASSWORD [password];`
   -`postgres=# ALTER USER catalog CREATEDB;`
   -`postgres=# CREATE DATABASE catalog with OWNER catalog;`
   -`postgres=# \c catalog`
   -`catalog=# REVOKE ALL ON SCHEMA public FROM public;`
   -`catalog=# GRANT ALL ON SCHEMA public TO catalog;`
   -`catalog=# \q`
   -`exit`
   
-Again  edit your
    - `__init__.py`
    - `database_setup.py` 
    -`seed.py`
by using command `sudo nano [file you want add]` and change the database engine from `sqlite://catalog.db` to `postgresql://username:password@localhost/catalog`
- Again restart  apache by typing ` sudo service apache2 restart`
 ***
 #### Authenticate login through google:
 you can find all steps you need [here](https://github.com/Heba-ahmad/FSND-P5-walkthrough/blob/master/Linux-Server-Configuration-p5-walkthrough.pdf)  even with pictures  by seeing step `13.2`
 ***
### Running the Item catalog application in server :
IF you close your terminal do as following
- `cd .ssh`
- login with the grader account using ssh type `ssh -i ~/.ssh/udacity.rsa grader@[your public ID] -p 2200 `you log in as `grader@ip-[your private IP]:~$`
- `cd /var/www/catalog/catalog`
-run `python database_setup.py`
-run `python seeder.py`
-run run `python __init__.py`
- open your browser and type the hostname 
***
#### References:
these references help me a lot to do my project and write README file I really appreciate their hard work 
- Full Stack Web Developer Nanodegree Program (https://www.udacity.com/)
- https://github.com/mulligan121/Udacity-Linux-Configuration/blob/master/README.md
- https://github.com/Heba-ahmad/FSND-P5-walkthrough/blob/master/Linux-Server-Configuration-p5-walkthrough.pdf
- https://github.com/chuanqin3/udacity-linux-configuration
- https://github.com/iliketomatoes/linux_server_configuration
