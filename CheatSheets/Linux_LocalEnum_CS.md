

```powershell

# ss command, which stands for socket statistics , and can be used to check which ports are listening locally on a given machine. 
ss -tlpn # -p for processes
ss -tln # shows port numbers (-n : no service name)
ss -tl  # shows service name
# -l: Display only listening sockets.
# -t: Display TCP sockets.
# -n: Do not try to resolve service names.
# -p for processes


```