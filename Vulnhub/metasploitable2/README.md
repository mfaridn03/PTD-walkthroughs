# Exercise 1: Metasploitable 2

Host discovery

![Text Description automatically generated](media/267ac593203f8aa65661275119d0f44f.png)

nmap full scan: nmap -A –Pn 192.168.50.17![A screenshot of a computer Description automatically generated with medium confidence](media/0247a69be5ce265c16cdb3fad6feff4b.png)

run dirb on port 80

![Text Description automatically generated](media/501ab63c05cb8568f055e751b2158e9e.png)

/dav/ directory exist. Run davtest on it to see if it’s vulnerable to WebDAV exploit (which it is)

![Text Description automatically generated](media/1e1061e1dc10c482fcdb945562fb81f2.png)

php upoad & execution works. Upload a php reverse shell and run it

![Text Description automatically generated](media/b2ef76fbbb07307349c6efaafd65289e.png)

Upload file using cadaver

![](media/8c3f96ec6f0a717920aaa52d6c3e4e67.png)

Open in web: 192.168.50.17/dav/revshell.php

![Graphical user interface, text, application, Teams Description automatically generated](media/f85ea3f4246da17511080605bc68ba03.png)

Login to user msfadmin (password: msfadmin) as shown on website

![](media/b516ced24293d814f4d13daaca542829.png)

Run a python httpserver containing linpeas so that we can wget it onto target machine

![](media/38a6e444499b72342924d6a4ddf657b1.png)

![Text Description automatically generated](media/1677a4ce48e9dc45fd857ac95f702b45.png)

Run linpeas and look for any possible entry points

![Text Description automatically generated](media/4ef9fc8ac6c4fde4a6c6e775466ecc7f.png)

misconfigured nfs share privilege escalation <https://steflan-security.com/linux-privilege-escalation-exploiting-nfs-shares/>

![](media/26b35486ed54124f1e46fc90690923d3.png)

![A screenshot of a computer Description automatically generated with medium confidence](media/0a5375b507bb0354b41b4914485815cf.png)

![Text Description automatically generated](media/2f6d381789e4852bb66d440710852421.png)
