




# NMAP Scripts:

```powershell

# Folder of NMAP scripts
cd /usr/share/nmap/scripts/

# Search for different scrips using script.db
cat /usr/share/nmap/scripts/script.db  | grep "\"vuln\""
# Categories: https://nmap.org/book/nse-usage.html

# Run a specific category of scripts:
sudo nmap -sV -p 443 --script "vuln" 192.168.50.124
' NOTE: --script requires -sV as nmap takes its output and make a request to vulners.com api.
' More information is here: https://nmap.org/nsedoc/scripts/vulners.html


# Testing for a specific vulnerability using namp
# Step 1: Download the related NSE script. Example:
https://github.com/RootUp/PersonalStuff/blob/master/http-vuln-cve-2021-41773.nse

# Step 2: Move it to scripts folder:
sudo cp /home/kali/Downloads/http-vuln-cve-2021-41773.nse /usr/share/nmap/scripts/http-vuln-cve2021-41773.nse

# Step 3:  update script.db
sudo nmap --script-updatedb

# Step 4: run it:
sudo nmap -sV -p 443 --script "http-vuln-cve2021-41773" 192.168.50.124


```