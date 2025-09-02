### Control Sockets
```bash
student@lin-ops$ ssh -MS /tmp/jump student@10.50.11.132
pass: meeMd337xc6j
#Next, ping sweep from jump box. we will use a /27 for example.
student@jump$ for i in {97..126}; do (ping -c 1 192.168.28.$i | grep "bytes from" &); done 
#Results:
64 bytes from 192.168.28.97: icmp_seq=1 ttl=64 time=0.187 ms
64 bytes from 192.168.28.100: icmp_seq=1 ttl=63 time=1.46 ms
64 bytes from 192.168.28.105: icmp_seq=1 ttl=63 time=1.54 ms
64 bytes from 192.168.28.111: icmp_seq=1 ttl=63 time=1.54 ms
64 bytes from 192.168.28.120: icmp_seq=1 ttl=63 time=1.61 ms
#Split screen
#Create dynamic port forward
student@lin-ops$ ssh -S /tmp/jump jump -O forward -D9050
#Now, scan with dynamic port forward 
student@lin-ops$ proxychains nmap 192.168.28.100,105,111
#Results:
Nmap scan report for 192.168.28.100
Host is up (0.00097s latency).
Not shown: 998 closed ports
PORT     STATE SERVICE
80/tcp   open  http #Verified HTTP
2222/tcp open  EtherNetIP-1 #Verified SSH

Nmap scan report for 192.168.28.105
Host is up (0.00092s latency).
Not shown: 997 closed ports
PORT     STATE SERVICE
21/tcp   open  ftp
23/tcp   open  telnet
2222/tcp open  EtherNetIP-1 #Verified SSH

Nmap scan report for 192.168.28.111
Host is up (0.00086s latency).
Not shown: 997 closed ports
PORT     STATE SERVICE
80/tcp   open  http
2222/tcp open  EtherNetIP-1
8080/tcp open  http-proxy

#Verify ports (Banner grab) on all hosts
proxychains nc 192.168.28.100 80
proxychains nc 192.168.28.100 80
etc...

#Set up port forwarding
ssh -S /tmp/jump jump -O forward -L1111:192.168.28.100:80 -L1112:192.168.28.100:2222 -L21111:192.168.28.105:2222
ss -ntlp #This will show you all open port connections if bind failure occurs 
ssh -S /tmp/jump jump -O cancel -L2111:192.168.28.105:2222 #This will cancel and close this port forward instance

#Navigate to webserver in firefox
127.0.0.1:1111
#######################################################################
```
### Notional
```bash
#Authenticate to new systems
ssh -MS /tmp/tl username@127.0.0.1 -p 1112
#Ping sweep new network
student@jump$ for i in {129..158}; do (ping -c 1 192.168.150.$i | grep "bytes from" &); done

192.168.150.128 UP

#Cancel and set up dynamic port forwarding
ssh -S /tmp/jump jump -O cancel -D9050
ssh -S /tmp/tl tl -O forward -D9050

#proxychains nmap
proxychains nmap 192.168.150.128
proxychains nc 192.168.128.150 80
proxychains nc 192.168.150.128 22
80 #HTTP
22 #SSH

#Set up new port forwarding
ssh -S /tmp/t1 -O forward -L3111:192.168.150.128:80 -L31112:192.168.150.128:22
#Recon found username2 and password, authenticate to new system
ssh -MS /tmp/t2 username2@127.0.0.1 -p 3112

#For web servers, run:
http-enum
#Example:
proxychains nmap --script=http-enum 192.168.28.100
