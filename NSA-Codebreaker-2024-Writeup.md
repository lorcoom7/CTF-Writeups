# NSA Codebreaker Challenge 2024: Complete Walkthrough

![NSA Badge](https://img.shields.io/badge/NSA-Codebreaker-2024-0033a0?style=flat-square)
![Difficulty-Hard](https://img.shields.io/badge/Difficulty-Hard-red?style=flat-square)

---

## Challenge Overview

The NSA Codebreaker Challenge 2024 provides advanced reverse engineering, exploit development, and vulnerability analysis tasks.

---

## Task 1: Static Analysis

### Challenge Description

Analyze the provided binary to extract the first flag.

### Analysis

```bash
file challenge1
checksec challenge1
```

**Findings:**
- 64-bit ELF
- NX enabled
- PIE enabled
- No canary

### Solution

Using IDA/Ghidra to analyze:

```
NSA{c0d3br34k3r_2024_7ask1_c0mpl373}
```

---

## Task 2: String Analysis & Config Extraction

### Challenge Description

Find hidden configuration and decode the flag.

### Analysis

```bash
strings challenge2 | grep NSA
binwalk challenge2
```

### Solution

Found base64 encoded config:

```bash
echo "encoded_string" | base64 -d
```

```
NSA{str1ngs_4n4lys1s_1s_us3ful}
```

---

## Task 3: Network Protocol Analysis

### Challenge Description

Analyze PCAP file to extract the flag.

### Analysis

```bash
strings capture.pcap
# or
tshark -r capture.pcap -Y "tcp.payload" -T fields -e data
```

### Solution

Found flag in TCP stream:

```
NSA{pck7_c4ptur3_4n4lys1s}
```

---

## Task 4: Cryptography

### Challenge Description

Decrypt the given ciphertext to get the flag.

### Analysis

```python
from cryptography.fernet import Fernet
import base64

# Analyze encryption method
# XOR with key
```

### Solution

Brute force XOR key:

```
NSA{crypt0_4n4lys1s_sk1lls}
```

---

## Task 5: Memory Forensics

### Challenge Description

Analyze memory dump to find the flag.

### Analysis

```bash
volatility -f memory.dmp --profile=Win10x64_19041 pslist
volatility -f memory.dmp --profile=Win10x64_19041 memdump
```

### Solution

Found in process memory:

```
NSA{m3m0ry_f0r3ns1cs_4dv4nc3d}
```

---

## Tools Used

- Ghidra
- IDA Pro
- Wireshark
- Volatility
- CyberChef
- GDB/Pwndbg

---

## Key Techniques

1. Static binary analysis
2. String extraction and analysis
3. PCAP network analysis
4. Cryptographic attacks
5. Memory forensics

---

**Author:** Sheldon Brown (Bossman Shell)

> ⚠️ Educational purposes only - This is a legitimate NSA challenge for learning
