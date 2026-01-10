

# 10.129.95.210 

![[Pasted image 20260108174458.png]]



It seems AD Domain Controller. The Domain is mentioned in the NMAP results of LDAP port 389: `DOMAIN htb.local`
Add `htb.local` to /etc/hosts file and also use it with Impacket to see if you can get any users:

# Impacket-GetNPUsers

![[Pasted image 20260109131731.png]]


use `Name the Hash` to find the hashcat mode:

![[Pasted image 20260109131849.png]]


# Crack the password using Hashcat


```powershell
hashcat -m 18200 svc_hash.txt ~/A/wordlists/rockyou.txt -D 1 --potfile-disable
```
![[Pasted image 20260109132010.png]]


#  First Creds:

svc-alfresco : s3rvice













