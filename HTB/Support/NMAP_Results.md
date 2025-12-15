

# initla


```
PORT     STATE SERVICE       VERSION                                                                                                                                        
53/tcp   open  domain        Simple DNS Plus                                          
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2025-12-15 22:55:11Z)
135/tcp  open  msrpc         Microsoft Windows RPC                                    
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn                            
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: support.htb0., Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds?                                                          
464/tcp  open  kpasswd5?                                                              
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0                      
636/tcp  open  tcpwrapped                                                             
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: support.htb0., Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped                                                             
5985/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)                  
|_http-server-header: Microsoft-HTTPAPI/2.0                                           
|_http-title: Not Found                                                               
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows                    
                                                                                      
Host script results:                                                                  
| smb2-security-mode:                
|   3:1:1:                                 
|_    Message signing enabled and required
| smb2-time:                                                                          
|   date: 2025-12-15T22:55:20
|_  start_date: N/A                                                                   
|_clock-skew: 2s                           

```


| Port        |          |
| ----------- | -------- |
| 135,139,445 | SMB      |
| 53          | DNS      |
| 88          | Kerberos |
| 3268        | lda[]    |
| 5985        | HTTP     |



# Start From easiest first - SMB


```powershell
smbclient -N -L //$ip  # Step 1: Worked!
smbclient -U '' -L //$ip
smbclient -U 'guest' //$ip 
smbclient -U 'anonymous' //$ip 

# Step 2: SMBmap to understand the access rights
smbmap -H HTB -u 'guest'

```
