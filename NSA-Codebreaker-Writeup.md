# NSA Codebreaker Challenge 2023: Task 1-3 Walkthrough

![NSA Badge](https://img.shields.io/badge/NSA-Codebreaker-0033a0?style=flat-square)
![Difficulty-Medium](https://img.shields.io/badge/Difficulty-Medium-yellow?style=flat-square)

---

## Challenge Overview

The NSA Codebreaker Challenge provides realistic reverse engineering and exploit development tasks.

---

## Task 1: Basic Reverse Engineering

### Challenge Description

Analyze the provided binary to extract the hidden flag.

### Analysis

```bash
file challenge1.bin
strings challenge1.bin
```

### Solution

Found the flag embedded in the binary strings:

```
NSA{brut3_f0rc3_1s_n0t_4lw4ys_7h3_b3s7}
```

---

## Task 2: Buffer Overflow

### Challenge Description

Exploit a vulnerable server to obtain the flag.

### Analysis

```bash
# Analyze binary
gdb challenge2
disassemble main
```

Buffer overflow vulnerability found in input handling

### Exploitation

```python
payload = b"A" * 64 + b"BBBB" + p64(0x401020)
```

---

## Task 3: ROP Chain

### Challenge Description

Bypass DEP/NX protection using Return-Oriented Programming.

### Analysis

```bash
# Find ROP gadgets
ROPgadget --binary challenge3
```

### Solution

Build ROP chain to call system("/bin/sh")

```
NSA{r0p_ch41ns_4r3_p0w3rful}
```

---

## Tools Used

- GDB
- Pwntools
- ROPgadget
- objdump
- Radare2

---

## Lessons Learned

1. String analysis for quick wins
2. Buffer overflow fundamentals
3. Return-Oriented Programming (ROP)
4. ASLR bypass techniques

---

**Author:** Sheldon Brown (Bossman Shell)

> ⚠️ Educational purposes only - This is a legitimate NSA challenge for learning
