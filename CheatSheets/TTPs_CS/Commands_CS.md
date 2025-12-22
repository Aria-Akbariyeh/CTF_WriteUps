

# Add IP mapping

```powershell.

echo "10.129.227.248 s3.thetoppers.htb" | sudo tee -a /etc/hosts
# -a appends
```

# Create a word list
```powershell
echo -e "daniel\njustin" | sudo tee file.txt
# -e Enables interpretation of escape sequences

# Alternatively:

echo "daniel" | tee -a users.txt
echo "justin" | tee -a users.txt
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

# using **sed** with **^1** referring to all lines starting with a "1", deleting them with **d**, and doing the editing in place with
sed -i '/^1/d' demo.txt


```


# Power Shell

```powershell
# Bypass the execution policy, which was designed to keep us from accidentally running PowerShell scripts.
powershell -ep bypass 

# Search for a specific type file (*.kdbx) in this example. 
Get-ChildItem -Path C:\ -Include *.kdbx -File -Recurse -ErrorAction SilentlyContinue
```


