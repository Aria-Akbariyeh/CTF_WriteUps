
# Port Forwarding with Socat



```powershell
# find if socat is installed
which socat

# if socat exists
socat -ddd TCP-LISTEN:2345,fork TCP:10.4.176.215:5432
### -ddd` (Verbosity level)
# -d = level 1: Print fatal errors
# -dd = level 2: Print errors and warnings
# -ddd = level 3: Print errors, warnings, and notices (most verbose for debugging)
# TCP-LISTEN:2345: Creates a TCP server listening on port 2345
# fork: Crucial option! For each incoming connection, socat spawns a new process/child to handle it
	# Without fork: Only one client can connect at a time
	# With fork: Multiple simultaneous connections are possible
# TCP:10.4.50.215:5432 (second-endpoint), Connects to IP address 10.4.50.215 on port 5432
# Complete picture: 
						# Client → [Port 2345 on this machine] → socat → [10.4.50.215:5432]
```


# Extra - Sshuttle:


```powershell
'sshuttle  requires root privileges on the SSH client and Python3 on the SSH server, so its not always the most lightweight option

# With this command executed after socat port forwaring:
sshuttle -r database_admin@192.168.50.63:2222 10.4.50.0/24 172.16.50.0/24

#  it should have set up the routing on our Kali machine so that any requests we make to hosts in the subnets we specified will be pushed transparently through the SSH connection:
# From attackbox you can access both networks now:
smbclient -L //172.16.50.217/ -U hr_admin --password=Welcome1234
```

