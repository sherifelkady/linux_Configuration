# Project 1: Linux Server Configuration
### by Sherif Elkady

#### a baseline installation of a Linux server and prepare it to host your web applications.

## Server Details
* Public IP : 165.227.144.54
* Server OS : Ubuntu 19.04 x64 
* SSH Port: 2200
* Server Provider: DigitalOcean
* Web Apllication: Item Catalog
* URL: http://165.227.144.54/


## Configure Linux server

#### Check and upgrade
###### check list 
``` 
sudo apt-get update 
```
###### upgrade list 
```
sudo apt-get upgrade
```
#### Configure the local timezone to UTC
``` 
sudo timedatectl set-timezone UTC
```


## Create a new user named grader
```
~ sudo adduser grader
```
## Give grader the permission to sudo
```
usermod -aG sudo grader 
```

## Create the RSA Key Pair For The grader by using the ssh-keygen
#### In the local machine
```
ssh-keygen 
```
###### Then choose where you save the key
```
/c/Users/shire/.ssh/id_rsa
```
#### In the Remote  machine
```
mkdir .ssh
```
###### then 
```
touch .ssh/authorized_keys
```
###### then past the public key in the authorized_keys file
```
sudo nano .ssh/authorized_keys
```

## Configure the Firewall 
#### first check firewall status
```
sudo ufw status
```
#### Deny ALL incoming
```
sudo ufw default deny incoming
```
#### Allow out going
```
sudo ufw default allow outgoing
```
#### Change ssh port from 22 to 2200
```
sudo nano etc/ssh/sshd_config # then change 22 to 2200
```
#### Allow tcp in 2200 port 
```
sudo ufw allow 2200/tcp
```
#### Active firewall
```
sudo ufw enable
```
## install postgresql
```
sudo apt install postgresql postgresql-contrib
```
#### Accessing a Postgres Prompt Without Switching Accounts
```
sudo -u postgres psql
```
#### Create New User
```
sudo -u postgres createuser --interactive # create user with name catalog
```
#### Creating a New Database
```
sudo -u postgres createdb catalog
```
#### Opening a Postgres Prompt with the New Role
```
sudo -u catalog psql
```
## Deploy a Flask Application
#### install nginx
```
sudo apt-get install nginx
sudo rm /etc/nginx/sites-enabled/default
```

#### install git 
```
sudo apt-get install git
```
#### CLone catalog project from private repo
```
git clone https://github.com/sherifelkady/catalog.git

```
#### install pip 
```
sudo apt-get install python3-pip 
```
#### install virtualenv 
```
sudo pip install virtualenv 
sudo virtualenv venv 
source venv/bin/activate

```
#### install packges
##### in local virtualenv 
```
pip freeze > requirements.txt
```
##### in remote machine
```
pip install -r requirements.txt
```

#### install gunicorn 
```
pip install gunicorn
gunicorn -w 3 run:app
```

