
```powershell

# Normal login
ssh username@$ip  
ssh username@$ip  -p2345 # on a specific port

# login with password (quick bruteforce)
sshpass -f <filename> ssh username@$ip  # password is stored in a file
sshpass -p <passowrd> ssh username@$ip  # dangerous as the pass is visible. 
 
# Login with SSH key
chmod 600 id_rsa # make sure the key has the right access
ssh -i id_rsa user@$ip # use -i flag

```



# Local portwarding with ssh

```powershell
# forward traffic from the local port 1234 to the remote server remote.example.com 's localhost interface on port 22
ssh -L 1234:localhost:22 user@remote.example.com     # Opens SSH session as well
ssh -L 1234:localhost:22 user@remote.example.com -fn # Backgrounds the shell
```

# Local port forwarding with an active session


Allow ~C to enter command mode in SSH for quick port forwarding:

```powershell

# ------  Requirement ---------- #
# If you don’t see a config file, just create it:
nano ~/.ssh/config
# Inside the file, add:
EnableEscapeCommandLine yes

# ------- Execution ------------ #
# With a establishied active ssh session
# Press Enter to go a new line.
# Then press '~' + C (Capital)
ssh> -L 8443:127.0.0.1:8443 
# -L local port forwarding
# <localport>:<localIP>:<forwardport>
```



