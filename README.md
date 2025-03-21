# Huntress CTF 2024 Write-ups

Detailed write-ups, solutions, and notes from the **2024 Huntress CTF**.  
These challenges cover real-world cybersecurity scenarios including **privilege escalation**, **malware analysis**, **OSINT**, **cryptography**, and more.

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

<details>
<summary><strong>Challenge Overview</strong></summary>

The **Base64by32** challenge required repeatedly decoding a string encoded in Base64 — 32 times, to be exact. The goal was to automate this process to retrieve the final hidden flag.

</details>

<details>
<summary><strong>Analysis & Methodology</strong></summary>

**Initial Analysis:**
- A single Base64 decode wasn't enough; the encoding was applied repeatedly.

**Automating the Decoding:**
```python
import base64

encoded_string = "your_base64_string_here"
for i in range(32):
    encoded_string = base64.b64decode(encoded_string).decode('utf-8')
    print(f"Decoding iteration {i+1}: {encoded_string}")

print("Flag:", encoded_string)
