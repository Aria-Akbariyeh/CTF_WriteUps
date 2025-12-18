

# GoBuster

```powershell

# ------------ Web ----------------- #
gobuster dir -u 192.168.50.20 -w /usr/share/wordlists/dirb/common.txt -t 5

# ------------ API ----------------- #
	# Step 1: Create a pattern file:
	{GOBUSTER}/v1
	{GOBUSTER}/v2
	
	# Step2: Gobust:
	gobuster dir -u http://192.168.50.16:5002 -w /usr/share/wordlists/dirb/big.txt -p pattern
	
	# Step3: Keep digging (e.g. if any username was found)
	gobuster dir -u http://192.168.50.16:5002/users/v1/admin/ -w /usr/share/wordlists/dirb/small.txt


```


