

# Directories

```powershell
ls -l # list
ls -a # hidden files
ls -la # list alll
ls -la | grep '^d' # show directories
ls -la | grep '^-' # show files
```
# Find Files and Directories

```powershell

# Find if a tool exists on a machine
which "toolname"
ls /usr/bin | grep "socat"


```



# Linux 

```powershell
 # open the current dir in explorer
open . 

# Find and replace
sed -i 's/release 7/release 8/g' 48793.py

# using **sed** with **^1** referring to all lines starting with a "1", deleting them with **d**, and doing the editing in place with
sed -i '/^1/d' demo.txt


```


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


