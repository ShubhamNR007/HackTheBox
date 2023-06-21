----------------------------------------------------
# Author -> Shubham Rannpise
----------------------------------------------------
# Mirai
# 20/3/2023
10.10.10.48

----------------------------------------------------
# nmap
----------------------------------------------------
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 6.7p1 Debian 5+deb8u3 (protocol 2.0)
53/tcp   open  domain  dnsmasq 2.76
80/tcp   open  http    lighttpd 1.4.35


after intercepting port 80 with burp we know there is dns pi.hole
add pi.hole to hosts

we land on administrator page of pi hole
http://pi.hole/admin/

----------------------------------------------------
# exploitation
----------------------------------------------------
since pi.hole is belong to raspberry I try default creds of raspberry pi to ssh it worked
pi:raspberry

----------------------------------------------------
# priv esc /flag
----------------------------------------------------
sudo su -- we have all roots privs already

root@raspberrypi:/root# strings /dev/sdb
