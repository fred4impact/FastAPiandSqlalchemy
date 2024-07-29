-- This apis Build with FastAPI SQL achemy 

-- To run this application, run
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

ENDPOINTS URLS:

### URLS ### 
## Get all events 
http://localhost:8000/events/ # to GET all events

## Get single events 
http://localhost:8000/events{event_id}  # to GET single events 

## Post events
http://localhost:8000/events/ # to POST or create new events

## Delete events
http://localhost:8000/events{event_id}

## Update
 http://localhost:8000/events{event_id}


-- Other stuff 

# Deployment Steps on AWS EC2

# THESE INSTAALTION WORKS 
# Steps
#Login into your AWS Console and create an EC2 choose ubuntu 
#create a security group ssh to you ip ,http port 80, https port 443 andcustom tcp port 8000
#SSH Into your ec2 start configuration

# Configuration

## 

    1  sudo apt upgrade -y
    2  sudo apt update
    3  sudo apt upgrade -y
    4  sudo apt install python3-pip -y
    5  sudo apt install git -y
    6  git clone https://github.com/fred4impact/FastAPiandSqlalchemy.git
    7  ls
    8  sudo apt install python3-virtualenv
   10  sudo pip3 install virtualenv
   11  virtualenv venv
   12  source venv/bin/activate
   13  pip3 install -r requirements.txt
   14  vi start.sh
   15  chmod +x start.sh
   16  sudo apt install supervisor -y
   17  sudo vi /etc/supervisor/conf.d/fastapi.conf
   18  sudo supervisorctl status
   19  sudo supervisorctl reread
   20  sudo supervisorctl update
   21  sudo vi /etc/supervisor/conf.d/fastapi.conf
   22  sudo supervisorctl status
   23  sudo supervisorctl reread
   24  sudo supervisorctl update
   25  sudo supervisorctl restart all
   26  sudo supervisorctl start uvicorn
  
 

############## START.SH. ##################
 

#!/bin/bash
source /home/ubuntu/fastapi/venv/bin/activate
exec uvicorn main:app --host 0.0.0.0 --port 8000


############# END ##############

# sudo vi /etc/supervisor/conf.d/fastapi.conf. add teh script below to the file ###

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

               
####. 

# WORKED. VIEW THE API WITH THE THE LINK BELOW CHANGE TEH IP TO TH EEC2 IP ADDRESS

http://YOUR_EC2_IPA_DDRESS:8000/docs#/



## Use these commands to check suoervisro logs 
sudo tail -f /var/log/supervisor/fastapi_access.log
sudo tail -f /var/log/supervisor/fastapi_error.log


### COMMAND TO START AND REATSRT THE SUPERVISOR

sudo supervisorctl restart uvicorn
sudo supervisorctl status
sudo supervisorctl reread
sudo supervisorctl update
sudo supervisorctl restart all
sudo supervisorctl start uvicorn

### end using the suoervisor the deploy 
