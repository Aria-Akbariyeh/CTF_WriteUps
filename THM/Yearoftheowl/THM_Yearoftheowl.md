# 10.201.117.172


1. start with fastscan:

nmap -Pn -n -F -sS -oN -vv fastScan.log 10.201.117.172

80/tcp   open  http          syn-ack ttl 126
139/tcp  open  netbios-ssn   syn-ack ttl 126
443/tcp  open  https         syn-ack ttl 126
445/tcp  open  microsoft-ds  syn-ack ttl 126
3306/tcp open  mysql         syn-ack ttl 126
3389/tcp open  ms-wbt-server syn-ack ttl 126

2. Port 80 is open. let's lunch a few things:

	-autorecon
	-full rustscan
	- fuzzing

3. Let's manual scan port 80


4. SMB 

no annoymous access: smbclient -L //10.201.117.172 -N 

enum4linx: enum4linux-ng $ip

not much found. 

5. mysql MariaDB 10.3.24

seems like we have unathorized access here


loot
------
404 page: Apache/2.4.46 (Win64) OpenSSL/1.1.1g PHP/7.4.10 Server at 10.201.117.172 Port 80


nmap: Apache/2.4.46 (Win64) OpenSSL/1.1.1g PHP/7.4.10


Avaialble serices
------------------
HTTP 80,443,5985, 47001
SMB: 139, 445
MySQL (mysql): 3306
RDP: 3389


----
Apache httpd 2.4.46 ((Win64) OpenSSL/1.1.1g PHP/7.4.10



