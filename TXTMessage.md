#Solving the "TXT Message" Challenge from Huntress CTF
**Author:** @JohnHammond  

---

## Challenge Description

> Hmmm, have you seen some of the strange DNS records for the `ctf.games` domain? One of them sure is odd...

**Hint:** The letters "od" in "odd" are highlighted in green, and the last "d" is in white.

---

### Introduction

For this Huntress CTF challenge, I needed to investigate the DNS records of `ctf.games` to find a hidden message or flag. The hint emphasized the word "odd," with "od" highlighted, suggesting that I should consider the `od` command or octal representation.

### Steps

#### 1. Retrieve the DNS Record

I started by using the `dig` command to retrieve the DNS records for `ctf.games`:

```bash
dig txt ctf.games
```

**Response:**

```
; <<>> DiG 9.10.6 <<>> txt ctf.games
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 35049
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; QUESTION SECTION:
;ctf.games.                      IN      TXT

;; ANSWER SECTION:
ctf.games.               14400   IN      TXT     "146 154 141 147 173 061 064 145 060 067 062 146 067 060 065 144 064 065 070 070 062 064 060 061 144 061 064 061 143 065 066 062 146 144 143 060 142 175"

;; Query time: 406 msec
;; SERVER: [Your DNS Server]
;; WHEN: [Current Date]
;; MSG SIZE  rcvd: 202
```

**Explanation:**

- The `dig` command is used to query DNS records. Here, I'm requesting the TXT records for `ctf.games`.
- In the `ANSWER SECTION`, I saw a TXT record containing a sequence of numbers enclosed in quotes.

#### 2. Save the TXT Record to a File

I copied the TXT record from the `ANSWER SECTION` and saved it to a file called `output` for easier manipulation.

#### 3. Analyze the Data

Looking at the sequence of numbers, I noticed:

- The numbers range from `060` to `175`.
- Numbers like `146`, `154`, `061` are indicative of **octal** numbers (base 8).

The hint emphasizing "od" (which is the name of a Unix command that can output data in octal) supported this observation.

#### 4. Decrypt the Data 

I used an online decrypter but I would like to add you can use the `awk` command to convert the octal numbers to their corresponding ASCII characters.

```bash
awk '{for(i=1;i<=NF;i++) printf("%c", strtonum("0"$i))}' output
```

**Explanation:**

- The `awk` scripting language is ideal for text processing and data extraction.
  - The hint pointed me toward octal encoding, so I needed a way to convert octal numbers to ASCII characters.
- **How the Command Works:**
  - `for(i=1;i<=NF;i++)`: Loops through each field (number) in the file `output`.
  - `strtonum("0"$i)`: Converts each number from a string to a numeric value, treating it as octal due to the leading zero.
    - Adding `"0"` ensures the number is interpreted in base 8.
  - `printf("%c", ...)`: Prints the ASCII character that corresponds to the numeric value.

#### 5. Retrieve the Flag

Running the above command outputs:

```
flag{14e072f705d45882401d141c562fdc0b}
```

**This is the flag!**

### Conclusion

By interpreting the sequence of numbers as octal values and converting them to ASCII characters using `awk`, I successfully decrypted the hidden message in the TXT record.

---

**Why I Did What I Did:**

- **Using `dig` to retrieve TXT records:** The challenge directed me to investigate strange DNS records for `ctf.games`. The `dig` command allows me to query DNS records, and specifying `txt` retrieves the TXT records, which often contain arbitrary text data.

- **Noticing the hint "od" in "odd":** The hint highlighted "od", suggesting the use of the `od` command or octal representation. This led me to consider that the numbers might be in octal format.

- **Copying the TXT record to a file:** Saving the data to a file makes it easier to manipulate and process with command-line tools.

- **Using `awk` for conversion:** `awk` provides a straightforward way to read each number, convert it from octal to decimal, and then to its ASCII character, all in one command.
---

# Flag

```
flag{14e072f705d45882401d141c562fdc0b}
```
