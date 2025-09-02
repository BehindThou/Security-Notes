### Results of nmap
```bash
Nmap scan report for 192.168.28.100
PORT     STATE SERVICE
80/tcp   open  http #HTTP
2222/tcp open  EtherNetIP-1 #SSH 

Nmap scan report for 192.168.28.105
Host is up (0.0019s latency).
Not shown: 997 closed ports
PORT     STATE SERVICE
21/tcp   open  ftp  #FTPD
23/tcp   open  telnet  #Telnet
2222/tcp open  EtherNetIP-1   #SSH

Nmap scan report for 192.168.28.111
Host is up (0.00072s latency).
Not shown: 997 closed ports
PORT     STATE SERVICE
80/tcp   open  http #HTTP
2222/tcp open  EtherNetIP-1 #SSH
8080/tcp open  http-proxy #ALTERNATE HTTP

Nmap scan report for 192.168.28.120
Host is up (0.00094s latency).
Not shown: 999 closed ports
PORT     STATE SERVICE
4242/tcp open  vrml-multi-use #SSH
```
### Creating tunnels
```bash
ssh -S /tmp/jump jump -O forward -L11111:192.168.28.100:80 -L11112:192.168.28.100:2222 -L21111:192.168.28.105:2222 -L31111:192.168.28.111:80 -L31112:192.168.28.111:2222 -L31113:192.168.28.111:8080 -L41111:192.168.28.120:4242
127.0.0.1 11111
