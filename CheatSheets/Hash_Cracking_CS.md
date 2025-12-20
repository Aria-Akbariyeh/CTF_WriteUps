

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