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
student@lin-ops$ ssh -S /tmp/jump jump -0 forward -D9050

