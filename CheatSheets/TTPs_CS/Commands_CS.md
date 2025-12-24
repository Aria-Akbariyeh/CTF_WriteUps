



#  Test what's the running shell

```powershell
(dir 2>&1 *`|echo CMD);&<# rem #>echo PowerShell
```


# Power Shell

```powershell
# Bypass the execution policy, which was designed to keep us from accidentally running PowerShell scripts.
powershell -ep bypass 

# Search for a specific type file (*.kdbx) in this example. 
Get-ChildItem -Path C:\ -Include *.kdbx -File -Recurse -ErrorAction SilentlyContinue
```


