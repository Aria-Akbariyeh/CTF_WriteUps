

```powershell

--------------- NETWORK ---------------
id # Check user privledges
ip addr # Check network interfaces
ip route # Understand how traffic is being directed



ss # Socket Statistics - shows all sockets 
ss -t # show TCP sockets 
ss -u # show UDP sockets 
ss -tl  # shows only sockets that the machine is listening on
ss -tln #  -n: no service name (show port numbers only) - sometimes needed
ss -tlpn # -p: show processes using the port
ss -tulpnu # combine all 
-----------------

```