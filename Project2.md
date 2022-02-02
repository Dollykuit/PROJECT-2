# Project 2


## Step 1 - Installing the Nginx Web Server

The below cmdlet installs the nginx server

     sudo apt update
     
     sudo apt install nginx
 
<img width="960" alt="update ngix" src="https://user-images.githubusercontent.com/98477745/152073573-ac582153-3944-40e6-abda-680d22837867.png">

     
<img width="960" alt="ngix install" src="https://user-images.githubusercontent.com/98477745/152073587-f665f471-953f-4273-a025-50e009dc9bcb.png">

we used the below cmdlet to confirm that nginx server is active

     sudo systemctl status nginx
     
     
<img width="960" alt="nginx status" src="https://user-images.githubusercontent.com/98477745/152073768-e4346101-afcb-4531-a8e5-b10031441faf.png">

Added an inbound rule to EC2 configuration to open inbound connection through port 80:

<img width="954" alt="Screenshot 2022-01-26 220946" src="https://user-images.githubusercontent.com/98477745/152074019-ff265c8b-c12e-4dc5-9548-da4583626f65.png">

Confirming if we can access our server locally

    curl http://localhost:80

<img width="960" alt="localhost 80 new" src="https://user-images.githubusercontent.com/98477745/152074820-53819be9-7081-452f-be8e-20302a21c12e.png">

Testing how the Nginx server can respond to requests from the Internet

<img width="960" alt="welcome to ngix" src="https://user-images.githubusercontent.com/98477745/152075020-0d9f1494-c83b-491f-8a4c-adf7136a98c8.png">

### Step 2 - Installing MySQL

The below cmdlet installs the Mysql into the server

    sudo apt install mysql-server
   
 <img width="960" alt="install mysql" src="https://user-images.githubusercontent.com/98477745/152075498-d09dba51-2dca-4a68-bdef-7dc01e0ef27a.png">
 
 The below cmdlet runs a security script that removes insecure default settings and lock down access to the database system
 
    sudo mysql_secure_installation
  
   
   <img width="960" alt="sudo mysql" src="https://user-images.githubusercontent.com/98477745/152076028-09f5f1da-f7f8-4efe-a786-f3f3466ed152.png">
   
   The below code verifies that we can log into the mysql server
   
     sudo mysql
     
     
  <img width="960" alt="sudo mysql" src="https://user-images.githubusercontent.com/98477745/152076194-bfa5d63c-6ba5-4cb9-b142-c328d53078e5.png">
  
  #### Step 3 Installing PHP
  
  The below cmdlet installs php-fpm (PHP fastCGI process manager) and php-mysql (PHP module to communicate with MySQL-based databases)
  
     sudo apt install php-fpm php-mysql
   
   <img width="960" alt="php" src="https://user-images.githubusercontent.com/98477745/152077052-c5a1ee30-216d-401a-b8b0-95385f1e32b7.png">
   
   
  ##### Step 4 - Configuring Nginx to use PHP processor
  
  Created a directory for projectLEMP using 'mkdir' command
  
    sudo mkdir /var/www/projectLEMP
    
  Assigned ownership to the directory with this variable $USER which still referenced the system user
  
    sudo chown -R $USER:$USER /var/www/projectLEMP
    
   created and opened a new configuration file in Nginx's sites available directory using ''nano'' command-line editor
   
     sudo nano /etc/nginx/sites-available/projectLEMP
     
   Pasted the below bare-bones configuration and save file using Ctrl+X to save it 
   
    #/etc/nginx/sites-available/projectLEMP

    server {
      listen 80;
      server_name projectLEMP www.projectLEMP;
      root /var/www/projectLEMP;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

    }
    
    

![5okayyyy](https://user-images.githubusercontent.com/98477745/152078939-0e3a683e-e21c-476d-98e0-c9b5b209b4ca.jpg)

We used the below to activate the configuration by linking to the config file from Nginx's sites-enabled directory

    sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/
    
  Wev tested the configuration for syntax error using the below cmdlet
  
     sudo nginx -t
  
  <img width="958" alt="syntax is okay" src="https://user-images.githubusercontent.com/98477745/152079363-9f38e05d-23d8-4713-8ad9-da273b0092be.png">
  
  Disabled default Nginx host that is currently configured to listen on port 80
  
     sudo unlink /etc/nginx/sites-enabled/default
  
  Reload the Ngix to apply the changes 
  
    sudo systemctl reload nginx
  
  We created an index.html file in the /var/www/projectLEMP to test if the new server block works
  
  
    sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html
     
   We access the public ip on the browser to get what we echoed 
   
   
   
   <img width="957" alt="hello lamp" src="https://user-images.githubusercontent.com/98477745/152080207-e82efd3d-aa54-48fe-aee6-d77a86578ef7.png">
   
   ###### Step 5 - Testing PHP with Nginx
   
   Created a test PHP file in the document root using nano text editor, this is to validate that nginx can correctly hand php file off to the PHP processor 
   
     sudo nano /var/www/projectLEMP/info.php
     
   Type or paste the following lines into the new file. This is valid PHP code that will return information about your server:
   
    <?php
    phpinfo();
    
    
   
   ![you](https://user-images.githubusercontent.com/98477745/152081055-3c3444a5-4b96-4e1b-a655-e8aac859f4a4.jpg)
   
   i accessed the page in my web browser by visiting my public IP address
  
  http://35.170.243.192/info.php

<img width="960" alt="php new" src="https://user-images.githubusercontent.com/98477745/152081377-0b7ba3e2-b935-4d84-a39a-fa97db34464d.png">

  
   


    
