# Automating LoadBalancer Configuration with Shell scripting

## Introduction
In this project we would automate the setup and maintenance of a load balancer by writing a shell script that when we run, all the task would be done for us automatically.

Automation helps speed up the deployment of services and reduces the chance of making errors in our day to day activity.

## Deploying and configuring the Webservers

## STEPS

-  STEP 1: Provision an EC2 instance running ubuntu 20.04.

- STEP 2: Open port 8000 to allow traffic from anywhere using the security group. A

- STEP 3: Connect to the webserver via the terminal using SSH client

- STEP 4: Open a file called install.sh paste the script below the file using the command  *sudo vi install.sh*


~

#!/bin/bash

####################################################################################################################
##### This automates the installation and configuring of apache webserver to listen on port 8000
##### Usage: Call the script and pass in the Public_IP of your EC2 instance as the first argument as shown below:
######## ./install_configure_apache.sh 127.0.0.1
####################################################################################################################

set -x # debug mode
set -e # exit the script if there is an error
set -o pipefail # exit the script when there is a pipe failure

PUBLIC_IP=$1

[ -z "${PUBLIC_IP}" ] && echo "Please pass the public IP of your EC2 instance as an argument to the script" && exit 1

sudo apt update -y &&  sudo apt install apache2 -y

sudo systemctl status apache2

if [[ $? -eq 0 ]]; then
    sudo chmod 777 /etc/apache2/ports.conf
    echo "Listen 8000" >> /etc/apache2/ports.conf
    sudo chmod 777 -R /etc/apache2/

    sudo sed -i 's/<VirtualHost \*:80>/<VirtualHost *:8000>/' /etc/apache2/sites-available/000-default.conf

fi
sudo chmod 777 -R /var/www/
echo "<!DOCTYPE html>
        <html>
        <head>
            <title>My EC2 Instance</title>
        </head>
        <body>
            <h1>Welcome to my EC2 instance</h1>
            <p>Public IP: "${PUBLIC_IP}"</p>
        </body>
        </html>" > /var/www/html/index.html

sudo systemctl restart apache2



~


- Close the file and type :wq on the vi editor

- STEP 5: Change the permissions on the file to make it an executable file using syntax *sudo chmod +x install.sh*


![ChangeToXfile](./img/3.install.png)


