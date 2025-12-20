
```powershell

'Default Username: administrator

# Remmina:
remmina -c 'rdp://username:password@192.168.1.100:3389'

# xfreerdp3:
xfreerdp3 /v:HostIP /u:Username /p:Password +clipboard /dynamic-resolution
# /f for fullscreen, but it can be problematic in VM


#Potential Errors
# Older versions of Windows don't support modern TLS/NLA security portocol which is the default for remmina and xfreerdp3
# THerefore, you might get the following error:
[17:38:28:011] [122966:0001e058] [ERROR][com.freerdp.crypto] - [freerdp_tls_handshake]: BIO_do_handshake failed
[17:38:28:011] [122966:0001e058] [ERROR][com.freerdp.core] - [transport_default_connect_tls]: ERRCONNECT_TLS_CONNECT_FAILED [0x00020008]


# in that case, change the security settings in remmina or use the following tools/commands:
# use /sec:rdp
xfreerdp3 /v:HostIP /u:Username /p:Password /sec:rdp

# or use rdesktop (pre-installed on kali by default)
rdesktop -u username -p password $IP

 