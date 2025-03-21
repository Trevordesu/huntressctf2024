# CTF Challenge Write-Up: QR Code Challenge

## Challenge Overview
The QR Code challenge provided a raw string of data that resembled the contents of a PNG file. The task was to reconstruct the PNG image and extract the hidden flag.

---

## Analysis and Methodology

### Initial Scanning of the QR Code
The QR code returned raw data in the format of a PNG file, starting with the typical PNG signature: `\x89PNG\r\n\x1a\n`. This was the first clue that the QR code encoded image data. The next step was to convert this raw data back into a valid PNG file for analysis.

### Reconstructing the PNG File
To rebuild the PNG image from the raw data, I took the following steps:

- **Step 1: Prepare the Raw Data for Writing**  
  The raw data provided by the QR code needed to be cleaned up to remove any extraneous characters and ensure it could be written in binary format. Here’s how I processed it:

```python
raw_data = b"\x89PNG\r\n\x1a\n\x00\x00\x00\rIHDR\x00\x00\x00'\x00\x00\x00'..."
with open("reconstructed_image.png", "wb") as file:
    file.write(raw_data)
```

This code takes the raw binary data, correctly formats it, and writes it to a `.png` file using the `wb` (write binary) mode.

- **Step 2: Analyze the Reconstructed Image**  
  After validating the image, I opened the reconstructed PNG file to visually inspect its contents. The image was another QR code, which, after being scanned, contained the flag, which was hidden within the PNG.

---

## Final Results
Through careful reconstruction of the PNG file from the raw data, I successfully extracted the flag:

```
flag{01c6e24c48f48856ee3adcca00f86e9b}
```

By reconstructing the PNG, I demonstrated the importance of recognizing file signatures in raw data and applying binary file handling techniques to recover hidden information. This approach allowed me to rebuild the image and retrieve the challenge's flag.
