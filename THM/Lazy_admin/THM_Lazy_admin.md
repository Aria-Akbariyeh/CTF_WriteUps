# 10.10.29.76


nmap 10.10.29.76 -sCV -Pn


http://10.10.29.76/content/


# Creds

manager
Password123


# methodology

gobuster and find /content

checking /content we know it's using sweetRice csm

checking vuln for sweetRice we find we can access content/inc/mysql_backup/ and download sql backup

checking the sql bak file we can find username:manager and password as a hash - cracked using crackstation: Password123

checked online pages and found /content/as is the login page for this csm

logged in and used media center to upload a webshell.php5

caught a shell and found the user flag

sudo -l shows there's a perl script that runs copy.sh

we have write access to copy.sh, so we just edit it: chmod +s /bin/bash


execute the initial perl script


finally: /bin/bash -p

Voila! 