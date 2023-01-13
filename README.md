# af2_sscjs
Server-Side Communication JavaScript

----------

AWS setup instructions

- generate Key Pairs and download it
- run WCS with at least 2 cores, otherwise experience is going to be the worst, and you'll be able to run only AVChat
- using PuTTY, generate .ppk file from .pem file
- login via cmd

ssh -i "mymy.pem" ec2-user@ec2-15-206-158-82.ap-south-1.compute.amazonaws.com

- login via WinSCP as root
- update WCS to the latest version

sudo -s  
cd /usr/local/FlashphonerWebCallServer/bin  
./webcallserver update yes

- update flashphoner.properties
- copy files /opt/affluxis/, /usr/local/FlashphonerWebCallServer/lib/custom/
- open ports TCP: 8081, 8444, 30000-33000, UDP: 30000-33000
- install ifstat

sudo yum install ifstat

- add affluxis user to WCS. you may want to specify a different password. this password is set at /opt/affluxis/conf/server.asc

ssh -p 2001 admin@localhost  
add app affluxis affluxis http://localhost:5000/affluxis  
add app-rest-method -a affluxis  
add user affluxis myPix  
passwd admin  
unPix  
update app -l http://localhost:5000/affluxis defaultApp  
update app -l http://localhost:5000/affluxis flashStreamingApp  
exit

- add services files to /etc/systemd/system/. Then execute the following commands

sudo systemctl daemon-reload  
sudo systemctl enable affluxis.service  
sudo systemctl enable stats.service  
sudo systemctl enable statsbw.service  

- add flashphoner user to ec2-user group

sudo usermod -G ec2-user flashphoner

- /opt/affluxis/ - owner/group flashphoner, 755, recursive
- /opt/affluxis/statsbw/ - owner/group root, recursive
- add symlink

affluxis  
/opt/affluxis/applications/wwwroot

- add SSL, you may either add your own SSL certificate or contact us at support@affluxis.com or Cell, Telegram, WhatsApp +852 5591 8235, for a 3 month subdomain SSL certificate for $6
- console url https://affluxis.com/console?a=yoursubdomain.affluxis.com&b=vhost_user&c=vhost_pass
- www url https://yoursubdomain.affluxis.com:8444/client2/affluxis/page-title-img.jpg
- services control

sudo systemctl status affluxis.service -l  
sudo systemctl start affluxis.service  
sudo systemctl stop affluxis.service  

sudo systemctl status stats.service -l  
sudo systemctl start stats.service  
sudo systemctl stop stats.service  

sudo systemctl status statsbw.service -l  
sudo systemctl start statsbw.service  
sudo systemctl stop statsbw.service  

cd /usr/local/FlashphonerWebCallServer/bin  
./shutdown.sh  
./startup.sh
