


```powershell
ftp -? # Help for ftp command
ftp> help # Help inside Ftp client
cd, dir, dir -a, ls. ls -a # -a is for hidden files
less file.txt # Display content
put <file> #uploading file
get <file> #downloading file
mget * # Download all visible files in the directory
mget .* # Donwload all hidden files in the directory

#  Use exclamation mark (!) to execute commands locally while in ftp:
ftp> !pwd # see your local directory
ftp> !cat user.txt # see the content of the downloaded user.txt
```


# FTP Enum

```powershell
# Check anonymous access
ftp anonymous@$ip

# Enumerate:
dir -a # hidden files
cd $RECYCLE.BIN # check recyclebin


# Check NMAP NSE scripts
locate .nse | grep ftp
nmap -p21 --script=<name> <IP>

# Bruteforce
hydra -L users.txt -P passwords.txt <IP> ftp #'-L' for usernames list, '-l' for username and vice-versa
hydra -l offsec -P /usr/share/seclists/Passwords/500-worst-passwords.txt <IP> ftp

# Check for vulnerabilities associated with the version identified.
```


`NOTE:`
- `Hidden Files: `Make sure to use `dir -a`  or `ls -a` to see the hidden files. 
- `RecycleBin:` Always check the Recycle bin `cd $RECYCLE.BIN`
- `Windows_LFIs`: Make sure to check important files if you have full access [[Important_Files_CS]]

# Potential Errors

```powershell

#if you see this kind of message,  229 Entering Extended Passive Mode (|||<port>|), toggle the Passive mode:
passive

```