
# NMAP

```powershell
# intial
sudo nmap -sCV -oN intial $ip

# full
sudo nmap -p- -oN all_ports $ip

# custom
sudo nmap -p customPorts -sCV -oN ports_numers $ip

```

# SMB

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

```