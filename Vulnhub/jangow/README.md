# Exercise 1: Jangow

nmap scan:  
![](media/212d547ccc3233d7a7cbe625af3229cd.png)

Webpage:  
![A screenshot of a computer Description automatically generated](media/80924391ea5f95f3ec0ffffd7724d7de.png)

site/ redirects to a main page:

![](media/c3d52620dbb49cbef8f2d4622c754bf6.png)

clicking on the buscar link leads here, seems like request

![A screenshot of a computer Description automatically generated](media/d83812f855098465c7c9a6a9ed4e7e69.png)

It executes bash commands

![](media/d35e1e546cb34ab1782c0074a485582f.png)

Get /etc/passwd

![](media/29130b938a9e138841fe08a092c7979d.png)

![Text Description automatically generated](media/a5b5a6c11eabab6086345c49a1300e80.png)

Located netcat, attempt to reverse shell but none are successful  
![](media/a4d0584eb2f91dbefd0cc41b060c0aae.png)

Seems like it is blocking outbound connections

![](media/95e1e99981d7c1ab34471dd143bb1086.png)

Using port 443 works, however. Probably a misconfigured firewall setting. To upload linpeas we need to first serve it on kali using http server on port 80 and download it using wget (that means ending reverse shell connection) from url

![](media/8c71ca4bf9d8e29b0e5898ca25747c7c.png)

Then, reverse shell again and execute linpeas

![Graphical user interface, text Description automatically generated](media/6d5603638292e69c384e54d7e7fad980.png)

This version of linux is vulnerable to the dirtycow exploit. We compile dirtycow on our kali, send it over onto target machine via port 443, and execute it.
