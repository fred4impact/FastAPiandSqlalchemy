-- This API is  Build with FastAPI SQL achemy 

-- To run this application, 
run
```
uvicorn app.main:app --reload
after startinf up go to this url

To run:  download the github repo here:
https://github.com/fred4impact/FastAPiandSqlalchemy
and  do the  below steps 

1.Download into your project directory 
2.create a virtual enviroments : python3 -m venv env
3.if pip is not installed on your enviroemtn install pip
4 .and run  pip3 install -r requirements.txt
5. to run the app open the terminal and run uvicorn app.main:app --reload
6. to view the api endpointsopen your browserand type http://localhost:8000/events/


```

-- deploymnet steps 


# How I Deploy and run the API endpoint on AWS EC2 usiing supervisor and uvicorn 

## Steps
```
Login into your AWS Console and create an EC2 choose ubuntu 
create a security group ssh to you ip ,http port 80, https port 443 andcustom tcp port 8000
SSH Into your ec2 start configuration

# Configuration

```
    -   sudo apt upgrade -y
    -   sudo apt update
    -   sudo apt upgrade -y
    -   sudo apt install python3-pip -y
    -  sudo apt install git -y
    -  git clone https://github.com/fred4impact/FastAPiandSqlalchemy.git
    -  ls
    -  sudo apt install python3-virtualenv
    -  sudo pip3 install virtualenv
    -  virtualenv venv
    -  source venv/bin/activate
    - pip3 install -r requirements.txt
    -  vi start.sh
    -  chmod +x start.sh
    -  sudo apt install supervisor -y
    -  sudo vi /etc/supervisor/conf.d/fastapi.conf
    -  sudo supervisorctl status
    -  sudo supervisorctl reread
    -  sudo supervisorctl update
    -  sudo vi /etc/supervisor/conf.d/fastapi.conf
    -  sudo supervisorctl status
    -  sudo supervisorctl reread
    -  sudo supervisorctl update
    -  sudo supervisorctl restart all
    - sudo supervisorctl start uvicorn
```
  
# content to go in start.sh
```
#!/bin/bash
source /home/ubuntu/fastapi/venv/bin/activate
exec uvicorn main:app --host 0.0.0.0 --port 8000
```


############# END ##############

# sudo vi /etc/supervisor/conf.d/fastapi.conf. 
Add the script below to the file  /etc/supervisor/conf.d/fastapi.conf. 

```
[program:uvicorn]
socket=tcp://localhost:8000
command=/home/ubuntu/fastapi/start.sh
process_num=4
directory=/home/ubuntu/fastapi
user=ubuntu
autostart=true
autorestart=true
stopasgroup=true
stdout_logfile=/var/log/supervisor/fastapi_access.log
stderr_logfile=/var/log/supervisor/fastapi_error.log
stopsignal=QUIT
```
               
####. 

## WORKED. VIEW THE API WITH THE THE LINK BELOW CHANGE TEH IP TO TH EEC2 IP ADDRESS

http://YOUR_EC2_IPA_DDRESS:8000/docs#/



# Use these commands to check suoervisro logs 
```
sudo tail -f /var/log/supervisor/fastapi_access.log
sudo tail -f /var/log/supervisor/fastapi_error.log
```

## COMMAND TO START AND REATSRT THE SUPERVISOR
```
sudo supervisorctl restart uvicorn
sudo supervisorctl status
sudo supervisorctl reread
sudo supervisorctl update
sudo supervisorctl restart all
sudo supervisorctl start uvicorn
```

## The end using supervisor and uvicorn to deploy to EC2

#Next update witl be for using the www-data user in /var/www/ directory 


# Another Way to setup

# Step-by-Step Guide to Set Up FastAPI on AWS EC2 with Supervisor in with www- user data and in /var/ww directory

# STEPS

 ```
  #SSH into Your EC2 Instance:
   ssh -i your-key.pem ubuntu@your-ec2-public-dns

 # Update Package Lists and Install Required Packages:
  
   sudo apt update
   sudo apt install python3.12 python3.12-venv python3-pip supervisor git -y
    
  #Create a Directory for Your FastAPI Application:
    
   25  sudo mkdir -p /var/www/fastapi
   26  sudo chown -R www-data:www-data /var/www/fastapi
   27  sudo chmod -R 755 /var/www/fastapi
   
   
   #Switch to www-data User:
   28  sudo -u www-data -H bash
   
  # Clone Your Repository
   cd /var/www/fastapi
   git clone https://github.com/fred4impact/FastAPiandSqlalchemy.git
  
  #Create and Activate the Virtual Environment:
    python3.12 -m venv venv
    source venv/bin/activate

  #Install Dependencies:
    pip install --upgrade pip
    pip install -r requirements.txt

   # Create the start.sh Script:
   vi  /var/www/fastapi/start.sh
  
   # Add the Following Content to start.sh:

   #!/bin/bash
   source /var/www/fastapi/venv/bin/activate
   exec uvicorn main:app --host 0.0.0.0 --port 8000

   # Make the Script Executable:
   chmod +x /var/www/fastapi/start.sh
   
   # Switch Back to ubuntu User:

    exit

   #Edit the Supervisor Configuration File:
    sudo vi  /etc/supervisor/conf.d/fastapi.conf
    
    # Add  the following content
    #  Use these content below in the  /var/www/fastpi   
  
  [program:uvicorn]
   socket=tcp://localhost:8000
   command=/var/www/fastapi/start.sh
   directory=/var/www/fastapi
   user=www-data
   autostart=true
   autorestart=true
   stdout_logfile=/var/log/supervisor/fastapi_access.log
   stderr_logfile=/var/log/supervisor/fastapi_error.log
   stdout_logfile=/var/log/supervisor/fastapi_access.log
   stderr_logfile=/var/log/supervisor/fastapi_error.log
   stopsignal=QUIT

  # Reload Supervisor Configuration:
   sudo supervisorctl reread
   
   # Update Supervisor Configuration:
   31  sudo supervisorctl update
   
   # Restart the Uvicorn Process:
   sudo supervisorctl restart uvicorn
   
   # Check the Supervisor Logs:
   sudo tail -f /var/log/supervisor/fastapi_error.log
   sudo tail -f /var/log/supervisor/supervisord.log

 ```
  
  ### View Your application on 
  http://52.200.77.115:8000/docs#/.  http:/TOUR_EC2_IP_ADDRESS:8000/docs#/

  ### YOU CAN CREATE A LOAD BALANCER ABD ROUT 53 NAME TO LINK TO YOUR api 
 
   Thanks






