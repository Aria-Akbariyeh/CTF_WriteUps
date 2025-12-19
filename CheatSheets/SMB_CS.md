

# SMB Methodology

```powershell

# Enum 
nxc smb $ip --shares
nxc smb $ip -u '' -p '' --shares             
nxc smb $ip -u 'guest' -p '' --shares       
nxc smb $ip -u 'anonymous' -p '' --shares

# Access
smbclient -N //$ip/sharename
smbclient -U 'guest/anounymous' //$ip/sharename

# Functionality 
get/put <file> # download/upload
more filename.txt # to view content

	# To DL all:
	mask "" # all files will be considered for download
	recurse ON # Recursive
	prompt OFF # Don't prompt
	mget * # Get All
	
#  

```

# Windows_SMB

```powershell

#-------- Get path of shares------:

# Method 1: 
wmic share get name,path

# Method 2:
	# Step 1: Get computer name:
	echo hostname
	echo %COMPUTERNAME%
	
	# Step 2: net view shares
	net view \\<computername>
	
# Method 3:
net share <sharename>




# ---------------------------------




net share
wmic share get name,path,status

# Get all SMB shares
Get-SmbShare

# Get detailed share information including permissions
Get-SmbShare | Get-SmbShareAccess

# Get shares with session information
Get-SmbOpenFile

# Get connected users to shares
Get-SmbSession


```
## RPC Enumeration

```powershell
rpcclient -U=user $IP
rpcclient -U="" $IP #Anonymous login
##Commands within in RPCclient
srvinfo
enumdomusers #users
enumpriv #like "whoami /priv"
queryuser <user> #detailed user info
getuserdompwinfo <RID> #password policy, get user-RID from previous command
lookupnames <user> #SID of specified user
createdomuser <username> #Creating a user
deletedomuser <username>
enumdomains
enumdomgroups
querygroup <group-RID> #get rid from previous command
querydispinfo #description of all users
netshareenum #Share enumeration, this only comesup if the current user we're logged in has permissions
netshareenumall
lsaenumsid #SID of all users
```



SMB
```powershell

sudo nbtscan -r 192.168.50.0/24 #IP or range can be provided

#--------------------------------------#
# --------- SMB Client ----------------#
#--------------------------------------#

# list the shares with anonymous access
smbclient -L //$ip -N #-L: list, -N: No password (this is no necessary actually)
smbclient -L //$ip
smbclient -L \\\\$ip


# Smbclient with creds
smbclient //server/share -U <username>
smbclient //server/share -U domain/username
smbclient //server/share -U domain/username --password 'passwordString'

# Login to a share annonymously
smbclient  //10.201.24.162/Users -N
# if the above command listed any lists, try to access it:
smbclient //10.201.48.118/<sharename>


# use more command to view text files
smb: \> more passwords.txt 
# Download files:
smb: \> get passwords.txt 
# Upload files (requires write access)
smb: \> put reverse_shell.asp 

#Within SMB session
put <file> #to upload file
get <file> #to download file

# Download all files from a share
mask "" # all files will be considered for download
recurse ON
prompt OFF
mget *


# ----------- NMAP ---------------

#NSE scripts can be used
locate .nse | grep smb
nmap -p445 --script="name" $IP

# general enum scripts - get shares info
sudo nmap -Pn -n  -script smb-enum* -p139,445 $ip  -oN nmap_smb_enum.log

# check vulnerabilities:
nmap -Pn -n  -script smb-vuln* -p139,445 $ip$ -oN nmap_smb_enum_vuln.log


#--------------- Mount a SMB share------------------
```shell-session
sudo mount -t cifs -o username=htb-student,password=Academy_WinFun! //ipaddoftarget/"Company Data" /home/user/Desktop/
#Make sure cifs is installed:
sudo apt-get install cifs-util

#-------------- SMBmap --------------------
smbmap -H <target_ip>
smbmap -H <target_ip> -u <username> -p <password>
smbmap -H <target_ip> -u <username> -p <password> -d <domain>
smbmap -H <target_ip> -u <username> -p <password> -r <share_name>


# ------------- Linux ----------------
enum4linux-ng $ip


#In windows we can view like this
net view \\<computername/IP> /all

```




```powershell

# CRACKMAPEXEC
crackmapexec smb <IP/range>
crackmapexec smb $ip -u '' -p '' --shares   #  can grap banner at least impa
crackmapexec smb $ip -u '' -p ''  
crackmapexec smb 192.168.1.100 -u username -p password
crackmapexec smb 192.168.1.100 -u username -p password --shares #lists available shares
crackmapexec smb 192.168.1.100 -u username -p password --users #lists users
crackmapexec smb 192.168.1.100 -u username -p password --all #all information
crackmapexec smb 192.168.1.100 -u username -p password -p 445 --shares #specific port
crackmapexec smb 192.168.1.100 -u username -p password -d mydomain --shares #specific domain
crackmapexec smb 10.201.70.3 # Banner grabbing | gets computer name, domain name, OS info, SMB info
crackmapexec smb 10.201.70.3 -d Relevant -u Bob -p '!P@$$W0rD!123' --shares
crackmapexec smb 10.201.70.3 -d Relevant -u Bob -p '!P@$$W0rD!123' --users
#bruteforce SMB
crackmapexec smb <MACHINE-IP> -u <username> -p /usr/share/wordlists/rockyou.txt
```


