
```powershell

# 1. Scan
sudo nmap -sC -sV 10.129.36.70 -oN inital_scan
sleep 300; sudo nmap -Pn -n -p- -sCV -oN full_scan 10.129.36.70

# 2. Footprint

PORT      STATE    SERVICE        VERSION                                                                                                                                   
135/tcp   open     msrpc          Microsoft Windows RPC                                                                                                                     
139/tcp   open     netbios-ssn    Microsoft Windows netbios-ssn                                                                                                             
445/tcp   open     microsoft-ds   Windows 7 Professional 7601 Service Pack 1 microsoft-ds                                                                                   
1122/tcp  filtered availant-mgr                                                                                                                                             
6547/tcp  filtered powerchuteplus                                                                                                                                           
6666/tcp  filtered irc                                                                                                                                                      
8194/tcp  filtered sophos                                                                                                                                                   
30000/tcp filtered ndmps                                                                                                                                                    
49152/tcp open     msrpc          Microsoft Windows RPC                                                                                                                     
49153/tcp open     msrpc          Microsoft Windows RPC                                                                                                                     
49154/tcp open     msrpc          Microsoft Windows RPC                                                                                                                     
49155/tcp open     msrpc          Microsoft Windows RPC                                                                                                                     
49156/tcp open     msrpc          Microsoft Windows RPC                                                                                                                     
49157/tcp open     msrpc          Microsoft Windows RPC                                                                                                                     
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows      


# 3. SMB Scan:


```
