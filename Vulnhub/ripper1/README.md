# Exercise 1: Ripper 1

IP: 192.168.50.19  
![](media/480bf9f55864bf3f4357ad3fa0be6171.png)

Webpage at port 80 shows a default ubuntu welcome page

Webpage at port 10000

![Graphical user interface, text, application Description automatically generated](media/2e33ef7f3e7e0d34ab9b6040c0968dfa.png)

Edit /etc/hosts file:

![Text Description automatically generated](media/258993b4072e5948fb68563d91ccad78.png)

![Graphical user interface, website Description automatically generated](media/afce8c3dbc80af2adc2c17505b6d1afc.png)

Default login is admin:admin which didn’t work

Running nmap vuln script:  
![Text Description automatically generated](media/1cfc397f5f09b379120296fa7bfbd73d.png)

Search on msfconsole

![Graphical user interface, text Description automatically generated](media/84a9e0128a1ba667543a0ddb1700d35f.png)

Exploit \#5 looks interesting, seems like there is a password_change.cgi webpage

![Word Description automatically generated with medium confidence](media/b231b73f2febf652ec5c933819284b12.png)

Visiting it looks like this. Seems like root is a user. We can now try to bruteforce our way into webmin

![A screenshot of a computer Description automatically generated](media/4a8db40ac8a30fb37aeea4bb80d4f17f.png)

![](media/69393d8847eb8822f0b2ab9396f3a25d.png)

hydra -l root -P rockyou.txt -S 192.168.50.19 https-post-form "/session_login.cgi:user=\^USER\^&pass=\^PASS\^:F=failed" -s 10000 -f

However, upon entering the credentials root:marie, the login page still shows login failed error![Background pattern Description automatically generated with medium confidence](media/5077ef64ff59328c1cb8c38f964695c9.png)

So I decided to run hydra again but with users from users.txt:  
![](media/d9946294d6090673e22bf722384154c8.png)

Obviously, there is something wrong. At this point I gave up for entry points, so I followed a walkthrough: <https://resources.infosecinstitute.com/topic/ripper-1-vulnhub-ctf-walkthrough/>

Seems like there is a /rips/ directory which I could not find using dirb

![](media/e765144746636e386d30fc3a56e046cd.png)

scan /var/www

![Graphical user interface, text, application, chat or text message Description automatically generated](media/fb2c40743c92c493a70be4c3f3f3f1b9.png)

![A screenshot of a computer Description automatically generated](media/1db79e6a794765081d1abe5139360215.png)

secret.php exists, containing login details. Tried on webmin login page which did not work.

Tried it on ssh and it worked

![Text Description automatically generated](media/e4f32674b5cbef57632310c673b2fe12.png)

Secret file at /mnt/secret.file which is the password to user ‘cubes’:  
![Text Description automatically generated](media/e400ab763d4cb5af74b2b2b081d2429c.png)

Bash history reveals a file where the webmin login details are potentially saved

![](media/b0a79f6eb76ead6dd7307ff70ab4517c.png)

In the contents of /var/webmin/miniserv.log lies the webmin admin

![Graphical user interface, text Description automatically generated](media/fb0cb57dddf9cd5bf082f09c27385391.png)

Clicking on terminal icon on the sidebar opens a terminal where we are logged in as root

![Text Description automatically generated](media/63abee09857bc3ffc4df2bbd8230792e.png)
