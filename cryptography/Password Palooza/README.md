## Password Palooza - PatriotCTF

### Description

We were tasked with infiltrating a hacker's network to bring down their team. During the investigation, we found the manager's password hash. OSINT revealed that the password was previously leaked in a popular breach, with **two extra random digits appended**.

* **Hash:** `3a52fc83037bd2cb81c5a04e49c048a2`
* **Password pattern:** `<leaked_password><two digits>`
* **Wordlist used:** `rockyou.txt`

### Approach

1. **Identify the hash type**
   The hash appeared to be an MD5 hash, so we treated it as such (`-m 0` in hashcat).

2. **Consider the password pattern**
   Since the password consists of a leaked password plus two digits, we could either:

   * Use **mask attack** in hashcat (`?d?d` for two digits)
   * Or manually append numbers in a script to every word from a known wordlist.

3. **Using hashcat** (if OpenCL/GPU available)
   Command example:

   ```bash
   hashcat -m 0 -a 6 hash.txt rockyou.txt ?d?d
   ```

   Here, `-a 6` performs a **wordlist + mask attack**, appending every combination of two digits to each word in the list.

4. **Alternative approach (CPU / script)**
   If GPU acceleration is not available, a simple Python script can append `00`–`99` to each word and check MD5 hashes against the target.

### Solution

After running the attack, the password was found:

```
mr.krabbs57
```

### Flag

```
pctf{mr.krabbs57}
```

