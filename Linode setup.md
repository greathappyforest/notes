# Linode setup
-------------------------




Follow Linode document 1 and 2 step
Link below

after install Nodejs/git/pm2 

ssh-keygen -t rsa -C "122391887@qq.com"

Install PM2 so you can run the app as a process

sudo npm install pm2 -g
pm2 start index.js


========================================================
install mongodb in 16.04
```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927
sudo echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.2 multiverse" | sudo tee 
```
/etc/apt/sources.list.d/mongodb-org-3.2.list
```
sudo apt-get update
sudo apt-get install -y --allow-unauthenticated mongodb-org
sudo nano /etc/systemd/system/mongodb.service
```
_________________
paste in '/etc/systemd/system/mongodb.service'
```
[Unit]
Description=High-performance, schema-free document-oriented database
After=network.target

[Service]
User=mongodb
ExecStart=/usr/bin/mongod --quiet --config /etc/mongod.conf

[Install]
WantedBy=multi-user.target
```

_________________________
```
sudo systemctl start mongodb
sudo systemctl status mongodb
sudo systemctl enable mongodb
mongo
use admin
db.createUser({user:"admin", pwd:"admin123", roles:[{role:"root", db:"admin"}]})
mongo -u admin -p admin123 --authenticationDatabase admin
sudo systemctl restart mongodb
```
Note:use 'xx' database 
db.createUser({user:"abc", pwd:"abc123", roles:[{role:"dbAdmin", db:"xx"}]})
var db = mongojs('mongodb://abc:abc123@127.0.0.1:27017/wfucsd?authSource=wfucsd', ['lotteries']);





=======================================================================

remote connect

firewall rule
```
sudo iptables -A INPUT -p tcp --destination-port 27017 -m state --state NEW,ESTABLISHED -j ACCEPT
sudo iptables -A OUTPUT  -p tcp --source-port 27017 -m state --state ESTABLISHED -j ACCEPT
```

check iptable
```
iptables -L
sudo netstat -tulpn | grep 27017
```

sudo nano /etc/mongod.conf
change:
```
bind_ip = 0.0.0.0
sudo systemctl stop mongodb
sudo systemctl start mongodb
```
Then check fire rule again
```
sudo netstat -tulpn | grep 27017
```
Make sure its 0.0.0.0:27017
```
tcp        0      0 0.0.0.0:27017           0.0.0.0:*               LISTEN  
```


After that mongodb will be able to connected remotely

mongodb://admin:admin123@127.0.0.1:27017/?authSource=admin


======================================================


Ref link: Note under youtube link
Getting Started with Linode
https://www.youtube.com/watch?v=CMEGih45sUQ&t=1s
Securing Your Server
https://www.youtube.com/watch?v=_5CGdA6Mjgc

Deploy Node.js App To Digital Ocean Server
https://www.youtube.com/watch?v=RE2PLyFqCzE

How to Install MongoDB on Ubuntu 16.04
https://www.youtube.com/watch?v=jSxeKxWQIGc

