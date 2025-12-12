# 10.201.104.21
---------------


1. Connect VPN. 
2. set /etc/hosts
3. set variables
4. Visual enumeration
5. Check the source code
6. Checked Robots.txt
7. Check usual directories or clues
8. directory discovery
8. Scan ports
10. Vulnerability scanning. we see port 22 - 

11. Have you enumerated everything??????
12. HAVE YOU?
13. DO IT AGAIN
---------------------
11. Attack 
12. Preveldge escalation



# loot:
--------
404 page: Apache/2.4.29 (Ubuntu) Server at 10.201.24.159 Port 80
source code: <!-- john, please add some actual content to the site! lorem ipsum is horrible to look at. -->
robots.txt ->  found path /uploads/


Found a username john in the sourcecode. 
Created a userlist.
Robots.txt led me to /uploads/ directory where I found a password dictionary.
Found port 22 hosting SSH after port scanning
We have a user, passwords, and the only ssh service looking at us, begging to be bruteforced. 
Well, that shit was a rabbit hole it seems. 
re-enumerated and found /secret directory after directory scanning, it conained a RSA hash

>>> ssh2john RSA_KEY > hashedkey

>>> john hashedkey -w=loot/dict.lst

>>> ssh john@10.201.104.21 -i RSA_KEY

>>> gobuster dir -w  ~/wordlists/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt -u 10.201.79.100 

>>> RSA hash