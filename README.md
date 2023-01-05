# af2_sscjs
Server-Side Communication JavaScript

----------

AWS setup instructions

- update wcs to the latest version

cd /usr/local/FlashphonerWebCallServer/bin
./webcallserver update yes

- update flashphoner.properties
- copy files (/home/ec2-user/affluxis/, /usr/local/FlashphonerWebCallServer/lib/custom/)
- ports (TCP: 8081, 8444, 30000-33000, UDP: 30000-33000)
- install/update ifstat

sudo yum install ifstat

- install dotnet

sudo rpm -Uvh https://packages.microsoft.com/config/centos/7/packages-microsoft-prod.rpm
sudo yum install aspnetcore-runtime-6.0

- add affluxis user to wcs. you may want to specify a different password. this password is specified at /home/ec2-user/affluxis/conf/server.sc

ssh -p 2001 admin@localhost
add app affluxis affluxis http://localhost:5000/affluxis
add app-rest-method -a affluxis
add user affluxis myPix
passwd admin
unPix
update app -l http://localhost:5000/affluxis defaultApp
update app -l http://localhost:5000/affluxis flashStreamingApp
exit

- add services

/etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl start affluxis.service
sudo systemctl enable affluxis.service
sudo systemctl start stats.service
sudo systemctl enable stats.service
sudo systemctl start statsbw.service
sudo systemctl enable statsbw.service

- /home/ec2-user/affluxis/statsbw/ - owner/group root, recursive
- add symlink

affluxis
/home/ec2-user/affluxis/applications/wwwroot

- add ssl

you may either add your own ssl certificate or contact us at support@affluxis.com or Cell, Telegram, WhatsApp +852 5591 8235, for a 3 month subdomain ssl certificate for $6

- execute these commands

usermod -G ec2-user flashphoner
chmod 755 /home/ec2-user/
chown -R flashphoner:flashphoner /home/ec2-user/affluxis/
chown -R flashphoner:flashphoner /home/ec2-user/ffmpeg/
sudo /usr/local/FlashphonerWebCallServer/bin/webcallserver set-permissions

- right-click /home/ec2-user/affluxis/ & /home/ec2-user/ffmpeg/ and set permissions to 755, subfolders
- right-click /home/ec2-user/affluxis/statsbw/ and set owner to root, permissions to 755, subfolders
- console url https://affluxis.com/console?a=demo.affluxis.com&b=vhost_user&c=vhost_pass
- wcs url https://demo.affluxis.com:8444/
- services control

sudo systemctl status affluxis.service
sudo systemctl stop affluxis.service
sudo systemctl start affluxis.service

sudo systemctl status stats.service
sudo systemctl stop stats.service
sudo systemctl start stats.service

sudo systemctl status statsbw.service
sudo systemctl stop statsbw.service
sudo systemctl start statsbw.service

cd /usr/local/FlashphonerWebCallServer/bin
./shutdown.sh
./startup.sh
