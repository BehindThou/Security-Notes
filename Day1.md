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
80/tcp   open  http
2222/tcp open  EtherNetIP-1

Nmap scan report for 192.168.28.105
Host is up (0.00092s latency).
Not shown: 997 closed ports
PORT     STATE SERVICE
21/tcp   open  ftp
23/tcp   open  telnet
2222/tcp open  EtherNetIP-1

Nmap scan report for 192.168.28.111
Host is up (0.00086s latency).
Not shown: 997 closed ports
PORT     STATE SERVICE
80/tcp   open  http
2222/tcp open  EtherNetIP-1
8080/tcp open  http-proxy


#Verify ports (Banner grab)

proxychains nc 192.168.28.100 80
Result: 
HTTP/1.1 400 Bad Request
Date: Tue, 02 Sep 2025 14:52:20 GMT
Server: Apache/2.4.29 (Ubuntu)
Content-Length: 313
Connection: close
Content-Type: text/html; charset=iso-8859-1

proxychains nc 192.168.28.100 80
#Result:
SSH-2.0-OpenSSH_7.6p1 Ubuntu-4ubuntu0.3

Protocol mismatch.

