# CTF Challenge Write-Up: Base64by32

## Challenge Overview
The **Base64by32** challenge required repeatedly decoding a string encoded in Base64, not just once but 32 times, to obtain the flag. The task was to automate this repetitive decoding process and retrieve the hidden flag.

---

## Analysis and Methodology

### Initial Analysis of the Encoded String
Upon receiving the encoded string, it became clear that running a single Base64 decode would not suffice. After examining the string’s structure and context, I realized that the encoding process had been applied multiple times.

### Automating the Decoding Process
To handle the repetitive decoding, I wrote a simple Python script. The script iteratively decoded the provided Base64 string 32 times, unraveling each layer of encoding until the flag was revealed.

---

## Python Script for Decoding
Here is the Python script used to automate the decoding process:

```python
import base64

encoded_string = "your_base64_string_here"
for i in range(32):
    encoded_string = base64.b64decode(encoded_string).decode('utf-8')
    print(f"Decoding iteration {i+1}: {encoded_string}")

print("Flag:", encoded_string)

This script continuously decodes the string until it reaches the final, fully decoded message containing the flag.

## Flag
After running the script and completing the 32 decoding iterations, the output revealed the hidden flag.
flag{8b3980f3d33f2ad2f531f5365d0e3970}
