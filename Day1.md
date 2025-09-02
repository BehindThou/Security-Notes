### Control Sockets
```bash
ssh -MS /tmp/jump student@10.50.11.132
pass: meeMd337xc6j
#Next, ping sweep. we will use a /27 for example.
for i in {97..126}; do (ping -c 1 192.168.28.$i | grep "bytes from" &); done 
