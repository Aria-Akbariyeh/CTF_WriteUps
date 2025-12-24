
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



