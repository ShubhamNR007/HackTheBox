----------------------------------------------------
# Author -> Shubham Rannpise
----------------------------------------------------
# Blocky
# 17/3/2023
10.10.10.37

----------------------------------------------------
# nmap
----------------------------------------------------
21/tcp   open   ftp     ProFTPD 1.3.5a
80/tcp   open   http    Apache httpd 2.4.18
8192/tcp closed sophos

----------------------------------------------------
# gobuster
----------------------------------------------------
/wiki                 (Status: 301) [Size: 307] [--> http://blocky.htb/wiki/]
/wp-content           (Status: 301) [Size: 313] [--> http://blocky.htb/wp-content/]
/plugins              (Status: 301) [Size: 310] [--> http://blocky.htb/plugins/]
/wp-includes          (Status: 301) [Size: 314] [--> http://blocky.htb/wp-includes/]
/javascript           (Status: 301) [Size: 313] [--> http://blocky.htb/javascript/]
/wp-admin             (Status: 301) [Size: 311] [--> http://blocky.htb/wp-admin/]
/phpmyadmin           (Status: 301) [Size: 313] [--> http://blocky.htb/phpmyadmin/]


http://blocky.htb/plugins/
download jar file and decompile it

we got user
 root: 8YsqfCTnvxAUeduzjNSXe22

now we can try on different places to see it is reused or not
it worked on phpmyadmin
http://blocky.htb/phpmyadmin/

i ran wpscan and got user notch 
tried notch with pass that found in jar file and it worked
and we can all commands as a root in notch's shell
GG