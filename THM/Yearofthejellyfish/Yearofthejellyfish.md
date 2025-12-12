
3.238.200.190

NMAP SCan reveals atlernative hostnames:

![[Pasted image 20251106115146.png]]


Add them all to hosts file

![[Pasted image 20251106115225.png]]


Accessing the machine using monitorr URL, shows that the website is using a monitoring app called

![[Pasted image 20251106115318.png]]


Searchsploit shows it's vulnerable:

![[Pasted image 20251106115551.png]]


Downlaoded the RCE 48980.py and fixed it, it gave me a reverse shell and I capatured the first flag:

![[Pasted image 20251106141424.png]]

Things needed to fix the python script:


1. The website had a self signed certificate. So needed to add the verify=false to http requests:
2. There was a cookie on the server called, IsHuman: 1. This cookie needed to be set.
	ck=`{"isHuman":"1"}` # added 
    resp = requests.post(url, headers=headers, data=data, `verify=False , cookies=ck`)
3.  Added the following on top to disable SSL certificate warnings:
    from urllib3.exceptions import InsecureRequestWarning 
	requests.packages.urllib3.disable_warnings(category=InsecureRequestWarning) 
4. The wesbite wasn't accepting .php exentsion, changed the extension to` .jpeg.phtml` 
5. Finally, I had to `use port 443` for the RevShell as 1337 was blocked

**Summary:**

The box has 3 websites hosted on 443, 8000, and 

PORT 21 is OPEN - Anonmous access to FTP is disabled
PORT 22 is OPEN - Anaonymous access to SSH is disabled
PORT 443 hosts the website
PORT 8000 has another website that receives ID: jelly:8000/ID
PORT 8096: A website hosted on 8096 -jellyfin
PORT 2000: Another SSH service is hosted on 22222


Attack Surfaces

- WebPenTest port jelly:443.
- Bruteforce IDs in the website hosted on 8000

If Stuck:
- Do a full  nmap port scan


# WebPenTesting

- The website is using FlatFile Pico by Gilbert Pellgoram

**Userlist:**
```powershell
Robyn
staff@robyns-petshop.thm

```



# Technologies

FlatFile PICO
Apache/2.4.29 (Ubuntu) Server at jelly Port 443



# Found

``` Powersell




```