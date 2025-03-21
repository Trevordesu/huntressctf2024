
# Huntress CTF 2024 Write-ups

Detailed write-ups and walkthroughs of challenges from the **Huntress CTF 2024**, showcasing real-world cybersecurity skills across privilege escalation, malware analysis, cryptography, and more. These are intended as part of my professional portfolio for hiring managers and technical reviewers.

---

## Challenges

### Base64by32

**Challenge Overview**  
The Base64by32 challenge required repeatedly decoding a string encoded in Base64, not just once but 32 times, to obtain the flag. The task was to automate this repetitive decoding process and retrieve the hidden flag.

**Analysis and Methodology**  
Upon receiving the encoded string, it became clear that running a single Base64 decode would not suffice. After examining the string’s structure and context, I realized that the encoding process had been applied multiple times.

**Python Script**
```python
import base64

encoded_string = "your_base64_string_here"
for i in range(32):
    encoded_string = base64.b64decode(encoded_string).decode('utf-8')
    print(f"Decoding iteration {i+1}: {encoded_string}")

print("Flag:", encoded_string)
```

**Flag**  
`flag{8b3980f3d33f2ad2f531f5365d0e3970}`

---

### Finders Fee

**Challenge Description**  
Escalate your privileges and uncover the flag.txt in the finder user's home directory.

**Solution**  
- Connected via SSH and performed enumeration.
- Found `find` binary with `setgid` to the `finder` group.
- Exploited this to run `find` with elevated group permissions and read the flag.

**Command Used**
```bash
find /home/finder/flag.txt -exec cat {} \;
```

**Flag**  
`flag{5da1de289823cfc200adf91d6536d914}`

---

### QR Code Challenge

**Overview**  
The QR Code challenge provided a raw string of data that resembled the contents of a PNG file. The task was to reconstruct the PNG image and extract the hidden flag.

**Steps Taken**  
- Recognized PNG signature in the string.
- Wrote binary data to a file using Python.
- Scanned the resulting QR code to retrieve the flag.

**Python Snippet**
```python
raw_data = b"\x89PNG\r\n\x1a\n..."
with open("reconstructed_image.png", "wb") as file:
    file.write(raw_data)
```

**Flag**  
`flag{01c6e24c48f48856ee3adcca00f86e9b}`

---

### Strange Calc

**Overview**  
A suspicious calc.exe triggered UAC prompts and dropped hidden `.jse` files on each run.

**Approach**  
- Identified repeated UAC prompts and `.jse` creation.
- Deobfuscated the `.jse` file using XOR via CyberChef.
- Found a payload that created a backdoor admin user.

**Instrumentation**
- Used WScript debug logging to analyze the payload.
- Extracted encoded string and manually decoded it.

**Final Flag**  
`flag{9922fb21aaf1757e3fdb28d7890a5d93}`

**Post-decryption Behavior**  
```cmd
net user LocalAdministrator [decoded_password] /add
net localgroup administrators LocalAdministrator /add
```

---

### TXT Message Challenge

**Challenge Description**  
DNS-based challenge hinting at `od`/octal encoding.

**Steps Taken**
- Queried DNS TXT record using `dig`.
- Found a sequence of octal values.
- Converted octal values to ASCII using `awk`.

**Command Used**
```bash
awk '{for(i=1;i<=NF;i++) printf("%c", strtonum("0"$i))}' output
```

**Flag**  
`flag{14e072f705d45882401d141c562fdc0b}`

---

## Closing Notes

These challenges demonstrated my ability to:
- Write automation scripts for encoding/decoding.
- Reverse engineer obfuscated malware.
- Investigate and exploit privilege escalation vectors.
- Leverage command-line tools for forensics and OSINT.

These experiences represent the kind of hands-on, practical cybersecurity work I aim to continue in a professional environment.
