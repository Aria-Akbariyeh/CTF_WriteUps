# 10.10.8.38


# services

php website port 80
ssh port 21


# Methodology

1.  Gobustered and found /panel page where you can upload files to /uploads
2.  uploaded .php webshell and rejected
3.  changed it to .php5 and accepted. 
4.  set up NC listener and executed the webshell. 
5.  upgraded to interactive session. 
6.  found user flag
7.  checked files with SUID set: find / -perm -4000 -type f 2>/dev/null 
8.  found /usr/bin/python
9.  went to GTFObins website and looked for python SUID exploit: python -c 'import os; os.execl("/bin/sh", "sh", "-p")'
10. rooted. found root flags
11. 


find / -perm -4000 -type f 2>/dev/null 