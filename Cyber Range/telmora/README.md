## Tel-Mora

Host discovery – 192.168.2.20

![](media/078e4bc097dfe34d39e093950d62eb7d.png)

Run quick and full nmap scan

![](media/66337a66e4d235553cc5b32e00b64c44.png)

![Text Description automatically generated](media/b556aed83fad10c0ad68045ebdd1af85.png)

While waiting for full scan to finish, visit webpage on port 80

![Graphical user interface, application Description automatically generated](media/23be693622b1831a94607a29dffcd0b7.png)

![Graphical user interface, text, application Description automatically generated](media/d25e505e5922dfbbca0422d8fffdb35c.png)

Run dirb on port 80:

![](media/f63bcc46887541d9e591ba5f5599e109.png)

![Text Description automatically generated](media/d3e568dd63971d72e4f51c4059a7e9b0.png)

robots.txt:  
![](media/b1f1f4884733ed256d52628913478d80.png)

nagios:

![Graphical user interface, text Description automatically generated](media/183c6d15a8a2ebd3b1bedb3d9a136e95.png)

Obviously, the username isn’t that

According to google, nagios is an application that monitors server infrastructure and network. Gaining access will likely allow access into the system

Default login details exist  
username: nagiosadmin  
password: PASSW0RD

which works.  
check for version:  
![Graphical user interface, text, application, chat or text message Description automatically generated](media/f447429f602767900fb943389fd2c97a.png)

Search Metasploit for nagios exploits  
![](media/9cd15f7d7efd7970570bd9629ce6292b.png)

Ignore all nagios xi entries. First one is also a nagios xi exploit as I have tried it. Second one didn’t work at all

Use exploit \#12 and set the options to these

![Text Description automatically generated](media/037e43738688f8c888cdb52deac5297d.png)

Run the exploit & upgrade shell

![Text Description automatically generated](media/e53ba495e1ea590778c037630a098fd0.png)

![Text Description automatically generated](media/347922a756185ad3cab0ca0fd583c05b.png)

get linPEAS and start python http server to download linpeas onto tel-mora.

![](media/41bf80a37bef3e54544dc9e461c25a8f.png)

Current user does not have write perms on current dir. So, look for directories that allow current user to write:

find / -type d -writable 2\> /dev/null

![](media/2bb5021465ea6e4745a180302406a62e.png)

Download linPEAS onto /tmp

![Text Description automatically generated](media/e3d5a8faff72d0d57c58a5d6a8233f58.png)

run linpeas.

/etc/passwd is writable. We can use this to escalate privileges

![](media/f7483d3fb75a7c52d69ad9b73ed649d2.png)

openssl passwd abc123

![](media/940c006f24796dd900797932a484fd66.png)

Copy /etc/passwd content into kali. Use vim to add a new user entry:  
root2:qcSDvGBvpZgDU:0:0:root:/bin/bash

![](media/31433fe542ae304ca87397982467eb55.png)

open python httpserver and wget the new password file

![Text Description automatically generated](media/ab10cde60193491c723cd4421a400d56.png)

note: here it saved as newpass.txt.1 since i messed up once. I simply removed newpass.txt and renamed newpass.txt.1 into newpass.txt

cat newpass.txt \> /etc/passwd

![Text Description automatically generated](media/1cd54bde5d64b1094393e06bc3c4f93b.png)

notice root2 at the bottom

Now we simply login with username root2 password abc123

![](media/5b12cb65e861f5d0bc6a67528b39e3e9.png)
