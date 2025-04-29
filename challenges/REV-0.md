## REV-0: Reverse Engineering
**Description:**
A light reverse engineering task involving decoding an array of integers to reconstruct the flag.
<details> <summary><b>Reveal Hidden Flag</b></summary> flag{2cf24dba5f} </details></br>

**Solution Summary:**
- Discovered an integer array in one of the files under `/ctf`.
- Converted the integer values to ASCII characters using a Python script.

**Exploitation Steps:**
1. Locate and review the two files under `/ctf` called `main.py` & `flag_data.json`.
2. The encoded flag is found under `flag_data.json` in the form of an integer array.\

    ![screenshot](../images/REV-0.jpg)

3. Decoded the flag using a python script I wrote:
   ```python
   #!/usr/bin/python3

   flag_nums = [102, 108, 97, 103, 123, 50, 99, 102, 50, 52, 100, 98, 97, 53, 102, 125]
   
   flag = ""
   for i in flag_nums:
       flag += chr(i)

   print(flag)
