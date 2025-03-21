# CTF Challenge Write-Up: Strange Calc

## Challenge Overview
The challenge involved a suspicious calculator application (`calc.exe`) that repeatedly triggered User Account Control (UAC) prompts without launching a visible application. After each UAC prompt was accepted, a hidden `.jse` (JavaScript Encoded) file was generated in the same directory as the calculator executable. Our objective was to investigate this behavior, analyze the `.jse` files, deobfuscate their contents using XOR, and extract the hidden flag.

---

## Analysis and Methodology

### 1. Initial Observation of the Application
Upon execution of `calc.exe`, the following behavior was observed:
- **Repeated UAC Prompts**: The application continuously requested UAC permissions. However, once the UAC prompt was accepted, no visible application opened.
- **Hidden File Creation**: Each time UAC was accepted, a hidden `.jse` file was created in the same directory as `calc.exe`. These files were encoded, making them unreadable upon initial inspection.
- **Observation**: This behavior suggested that `calc.exe` was being used as a dropper to execute hidden scripts via Windows Script Host (WSH), possibly with elevated privileges.

### 2. Identification of Obfuscated JavaScript
I examined one of the hidden `.jse` files and found that the JavaScript code was heavily obfuscated. Upon inspection, it became clear that the file was encrypted using XOR, a common technique for basic obfuscation. The `.jse` file contained an encoded string that could not be interpreted directly.

### 3. Deobfuscation Using Standard XOR
To deobfuscate the `.jse` file, we used CyberChef, a popular tool for performing cryptographic and encoding operations. We applied a standard XOR decryption technique to the file, using known XOR patterns. After testing several XOR keys, I successfully identified the key and decoded the obfuscated content into a readable format. The deobfuscated script revealed a function responsible for further decoding an embedded string, which was likely the flag for the challenge.

### 4. Decoding the Encoded String
The deobfuscated JavaScript included a base-64-like encoded string in the following format:
```
var m = "begin 644 -\nG9FQA9WLY.3(R9F(R,6%A9C$W-3=E,V9D8C(X9#<X.3!A-60Y,WT*\n`\nend";
```
This string was passed to a function `a(b)` responsible for decoding it. The function manipulated the string using bitwise operations and character code adjustments to decrypt the hidden content. To ensure the decryption process was functioning correctly, I introduced debugging statements within the script.

### 5. Instrumentation for Debugging
To monitor the decryption process, I added the following debug statements:
- **Logging Each Processed Line**: Inside the loop that processed the encoded string, I added:
  ```
  WScript.Echo("Processing line: " + f);
  ```
  This allowed me to see each line of the encoded string being processed.
- **Logging the Full Decoded String Before Trimming**: To inspect the string after decryption but before any trimming operations, we added:
  ```
  WScript.Echo("Decoded string before trimming: " + c);
  ```
  This allowed me to observe the fully decoded string, including any extraneous characters or padding.
- **Outputting the Final Decoded Flag**: After the decryption process was complete, I added:
  ```
  WScript.Echo("Decoded password: " + n);
  ```
  This printed the final decoded string, which contained the hidden flag.

### 6. Final Results
Upon running the instrumented script, the encoded string was successfully decrypted, and we retrieved the flag:
```
flag{9922fb21aaf1757e3fdb28d7890a5d93}
```

### 7. Script Behavior After Decoding
The deobfuscated script revealed that it attempted to create a new local administrator account using the following commands:
```
net user LocalAdministrator [decoded_password] /add
net localgroup administrators LocalAdministrator /add
```
This would create a backdoor administrator account, allowing persistent elevated access to the system. The script then launched `calc.exe` as a visual confirmation that the script had executed successfully.

---

## Conclusion
Through a combination of XOR decryption and script analysis, I successfully deobfuscated the `.jse` file and extracted the hidden flag:
```
flag{9922fb21aaf1757e3fdb28d7890a5d93}
```
This challenge demonstrated the use of basic XOR encryption combined with JavaScript obfuscation to hide sensitive information, which could be used for malicious purposes like privilege escalation. By applying XOR decryption and instrumenting the script with debugging statements, we were able to reverse engineer the script and recover the flag.

---

## Final Flag
```
flag{9922fb21aaf1757e3fdb28d7890a5d93}
```
