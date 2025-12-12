# 10.10.111.173


Potential:
JD@anthem.com
Jane Doe

James Orchard Halliwell
THM{AN0TH3R_M3TA}
THM{G!T_G00D}
THM{L0L_WH0_D15}
UmbracoIsTheBest!


admin email:SG@anthem.com


login to the server via rdp:
THM{N00T_NO0T}
xfreerdp3 /v:10.10.111.173 /u:sg /p:UmbracoIsTheBest! /dynamic-resolution +clipboard /cert:ignore

after loging in, check hidden files. you'll fine a backup folder with a restore.txt text file in it.
Apperently you can give permissions to it. So give it permission for SG user to read it.
It contains the password for Adminstrator

If you can go users folder, you'll find the  Administrator folder. you can browse it directly to get the final flag or rdp again:


xfreerdp3 /v:10.10.111.173 /u:Administrator /p:ChangeMeBaby1MoreTime /dynamic-resolution +clipboard /cert:ignore



cheers