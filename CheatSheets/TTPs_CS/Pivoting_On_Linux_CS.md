
```powershell

id # Check user privledges
ip addr # Check network interfaces
ip route # Understand how traffic is being directed
cat /etc/hosts # check IP mappings


# ----------------- Ping sweep ----------------- #
for i in {1..254} ;do (ping -c 1 10.4.176.$i | grep "bytes from" &) ;done
# Let's say we find the machine 10.4.176.215 accessable. 
# Scan ports with a range 

# ------------ Port Scan with Range ------------ #
for port in {1..1024}; do timeout 0.1 bash -c ">/dev/tcp/172.16.223.217/$port" 2>/dev/null && echo "Port $port: OPEN"; done
# If you found for example the following ports are open:
Port 22 is open
Port 5432 is open
# It may mean that you must look for eaither  SSH keys/creds or look into POSTGRE (port 5432) creds
# Always check the configuration files of the same application that you used for initial foothold. 
# Check /opt and /var folders for the same application as well. 

# ---------------- # Port Scan Common ports --------------- # 
ports="21 22 23 25 53 69 80 88 110 111 135 139 143 161 162 389 443 445 464 500 512 513 514 593 631 636 993 995 1194 1433 1521 1723 2049 3268 3269 3306 3389 5432 5900 6379 8000 8008 8080 8443 9200 11211 27017"; echo "Scanning common ports..." && for p in $ports; do timeout 0.2 bash -c ">/dev/tcp/172.16.223.217/$p" 2>/dev/null && echo "[+] Port $p - OPEN"; done 2>/dev/null
 
```


# One-liners

```powershell
# sweep for hosts with an open port 445 on the /24 subnet using Netcat
for i in $(seq 1 254); do nc -zv -w 1 172.16.50.$i 445; done
# assing the -z flag to check for a listening port without sending data, -v for verbosity, and -w set to 1 to ensure a lower time-out threshold.



```