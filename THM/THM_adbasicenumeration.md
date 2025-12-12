
```powershell

cat hosts.txt
10.211.11.10
10.211.11.20

# NMAP:
nmap -p 88,135,139,389,445 -sV -sC -iL hosts.txt -oN nmap.log

# 10.211.11.10 is the DC because it has ports 88 (Kerberos), 389 (ldap) and SMB(139,445) open. 

| smb-os-discovery: 
|   OS: Windows Server 2019 Datacenter 17763 (Windows Server 2019 Datacenter 6.3)
|   Computer name: DC
|   NetBIOS computer name: DC\x00
|   Domain name: tryhackme.loc
|   Forest name: tryhackme.loc
|   FQDN: DC.tryhackme.loc


> smbclient //10.211.11.10/UserBackups -N

#Found:
Flag: THM{88_SMB_88}
katie.thomas
2 minutes
rduke:Password1! 





```
