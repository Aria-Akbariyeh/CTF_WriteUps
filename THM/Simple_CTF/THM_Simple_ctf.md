# 10.10.228.64


# ports
21 FTP
80 Apache
2222 SSH

# methodology
1. gobuster finds /simple page
2. found a a exploit for simple cms: CVE-2019-9053
3. the exploit script was in python2, used online coverter to convert to python3
4. ran the script and it gave us md5 hash+salt:
salt: 1dac0d92e9fa6bb2
pass hash: 0c01f4468bd75d7a84c7eb73846e8d96
user: mitch


5. crached the hash+salt using hashcat: 

0c01f4468bd75d7a84c7eb73846e8d96:1dac0d92e9fa6bb2

hashcat -O -a 0 -m 20 0c01f4468bd75d7a84c7eb73846e8d96:1dac0d92e9fa6bb2 ~/wordlists/rockyou.txt


6. password: secret

7. ssh to the victim using ssh mitch@$ip -p 2222 ((((password: seceret))))

8. find user flags

9. sudo -l shows the user can run vim with sudo command

10. find the privilege esc. of vim from gtfobins


