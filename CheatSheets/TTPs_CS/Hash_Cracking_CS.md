

# Identify Hashes

```powershell
# name-that-hash:
nth --text 'hash'

# hashcat
hashcat --identify 'hash'

# hash-identifier
echo 'hash' | hash-identifier
# or use it interactively:
hash-identifier

#hashid
hashid 'hash'
```


# Crack hashes:

```powershell

# For Net-NTLMv1
john --format=netntlm --wordlist=rockyou.txt hash.txt
hashcat -m 5500 -a 3 hash.txt

# For Net-NTLMv2
john --format=netntlmv2 --wordlist=rockyou.txt hash.txt
hashcat -m 5600 -a 3 hash.txt

```



# JTR Transformation Scripts

```powershell

# find transformation scripts
find /usr/share/john -name "*2john*"
locate *2john*
find / -name "*2john*" 2>/dev/null | head -20

# find the right script, transform to hash, and crack with hashcat. Example:

# Step 1:
keepass2john Database.kdbx > keepass.hash

# Step 2: 
hashcat --help | grep -i "KeePass"

# Step 3: 
hashcat -m 13400 keepass.hash /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/rockyou-30000.rule --force



```


# Cracking ssh keys with passphrase

```powershell
cat note.txt
chmod 600 id_rsa
ssh2john id_rsa > ssh.hash

cat ssh.hash
id_rsa:$sshng$6$16$7059e78a8d3764ea1e883fcdf592feb7$1894$6f70656e7373682d6b65792d7631000000000a6165733235362d6374720000000662637279707400000018000000107059e78a8d3764ea1e883fcdf592feb70000001000..

# Within this output, "$6$" signifies SHA-512. As before, we'll remove the filename before the first colon. Then, we'll determine the correct Hashcat mode.

hashcat -h | grep -i "ssh"

john --wordlist=ssh.passwords --rules=sshRules ssh.hash

```


# Convert hashcat.rule file to JTR.config:

```powershell
sudo sh -c 'cat /home/kali/passwordattacks/ssh.rule >> /etc/john/john.conf'
```