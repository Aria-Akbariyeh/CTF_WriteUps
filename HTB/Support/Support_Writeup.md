

Scanning all the ports show we have SMB, an no other apparent clue. So let's enum it:

![[Pasted image 20251215183502.png]]


Access:

![[Pasted image 20251215183522.png]]


userinfo.exe.zip stands out. we download and review it:

![[Pasted image 20251215183904.png]]




The file is Mono/.Net assembly. So we need dotnet to run it. 

![[Pasted image 20251215184130.png]]


The config file doesn't have any clues:

![[Pasted image 20251215184005.png]]



Since the file is PE32 (wndows x32 bit exe file), we can try to run it with mono:


![[Pasted image 20251215204653.png]]



Since there's a connect error, let's check it with wireshark on all/`any` interfaces and look for `dns` requests:


![[Pasted image 20251215205710.png]]


We can see that the application send requests to support.htb, but DNS cannot find it. We need to update /etc/hosts file to address this problem. 

![[Pasted image 20251215205919.png]]

The problem solved:

![[Pasted image 20251215210125.png]]


Let's run the request again and look at the wireshark output for `tun0`:


![[Pasted image 20251215211815.png]]


![[Pasted image 20251215211844.png]]

![[Pasted image 20251215211911.png]]


We can see the username and password is transfered to the server in plain text. 

> `User:`  support\ldap
> `Password`: nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz



Let's try to see if any new info can be found using this creds:

```powershell
nxc smb $ip -u 'support\ldap' -p 'nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz' --shares   
```


![[Pasted image 20251215212841.png]]



Yes, we can see NETLOGON share is visible and accessible to us now. 

![[Pasted image 20251215213039.png]]

To be done later. seems so above my level right now. ...