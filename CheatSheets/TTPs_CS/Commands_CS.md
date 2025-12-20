

# Add IP mapping

```powershell.
echo "10.129.227.248 s3.thetoppers.htb" | sudo tee -a /etc/hosts
```

#  Test what's the running shell

```powershell
(dir 2>&1 *`|echo CMD);&<# rem #>echo PowerShell
```

# Linux utility

```powershell
 # open the current dir in explorer
open . 

# Find and replace
sed -i 's/release 7/release 8/g' 48793.py
```


# Power Shell

```powershell
powershell -ep bypass # bypass the execution policy, which was designed to keep us from accidentally running PowerShell scripts.
```

