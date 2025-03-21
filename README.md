# Huntress CTF 2024 Write-ups
Detailed write-ups, solutions, and notes from the **2024 Huntress CTF**. These challenges cover real-world cybersecurity scenarios including **privilege escalation**, **malware analysis**, **OSINT**, **cryptography**, and more.

---

## Table of Contents
1. [Overview](#overview)  
2. [Challenges](#challenges)  
   - [Base64by32](#base64by32)  
   - [Finders Fee](#finders-fee)  
   - [QR Code Challenge](#qr-code-challenge)  
   - [Strange Calc](#strange-calc)  
   - [TXT Message Challenge](#txt-message-challenge)  
3. [Tools & Techniques](#tools--techniques)  
4. [Lessons Learned](#lessons-learned)  
5. [Disclaimer](#disclaimer)

---

## Overview
This repository showcases my approach to each **Huntress CTF 2024** challenge. For each challenge, I include:
- A **summary** of the challenge
- My **methodology** (tools, commands, scripts)
- The **flag** (where allowed)
- Key **takeaways** for other learners

**Event Date**: 2024  
**Organizer**: [Huntress](https://www.huntress.com/)

---

## Challenges

### Base64by32
**Challenge Overview:**  
The Base64by32 challenge required repeatedly decoding a string encoded in Base64 — 32 times, to be exact.

**Methodology:**  
Automated the decoding process using Python:
```python
import base64

encoded_string = "your_base64_string_here"
for i in range(32):
    encoded_string = base64.b64decode(encoded_string).decode('utf-8')
    print(f"Decoding iteration {i+1}: {encoded_string}")

print("Flag:", encoded_string)
```

**Flag:**  
flag{8b3980f3d33f2ad2f531f5365d0e3970}

---

### Finders Fee
**Objective:**  
Escalate from user to finder and access `/home/finder/flag.txt`.

**Key Steps:**  
- Found `/usr/bin/find` had group `finder` and `setgid` bit.
- Used:  
```bash
find /home/finder/flag.txt -exec cat {} \;
```

**Flag:**  
flag{5da1de289823cfc200adf91d6536d914}

---

### QR Code Challenge
**Overview:**  
Raw data resembled a PNG image. Wrote it to a file and scanned the resulting QR code.

**Python Example:**  
```python
raw_data = b"\x89PNG\r\n\x1a\n..."
with open("reconstructed_image.png", "wb") as file:
    file.write(raw_data)
```

**Flag:**  
flag{01c6e24c48f48856ee3adcca00f86e9b}

---

### Strange Calc
**Overview:**  
`calc.exe` generated `.jse` scripts after UAC prompts.

**Key Techniques:**  
- Used CyberChef to XOR decode
- Injected logging into the script
- Found backdoor admin creation attempt

**Flag:**  
flag{9922fb21aaf1757e3fdb28d7890a5d93}

---

### TXT Message Challenge
**Overview:**  
Used `dig` to extract a DNS TXT record, which was octal-encoded.

**Command Used:**  
```bash
awk '{for(i=1;i<=NF;i++) printf("%c", strtonum("0"$i))}' output
```

**Flag:**  
flag{14e072f705d45882401d141c562fdc0b}

---

## Tools & Techniques
- Python, Bash, CyberChef
- dig, awk, find, netstat
- XOR decryption, QR analysis

---

## Lessons Learned
- Enumerate thoroughly
- Automate where possible
- Understand privilege context
- Inspect file signatures & metadata

---

## Disclaimer
These write-ups are from a public CTF event and shared for educational purposes only.
