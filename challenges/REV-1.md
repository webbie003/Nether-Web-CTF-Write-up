## REV-1: Flag Authentication
**Description:**
This challenge involved decrypting a file using a known encryption algorithm along with the provided key and IV values.
<details> <summary><b>Reveal Hidden Flag</b></summary> flag{735701f285} </details></br>

**Solution Summary:**
- Discovered the encryption algorithm used was AES-128-CBC.
- Both the key and IV were recoverable from the system or accompanying files.
- Used OpenSSL to manually decrypt the encrypted file.

**Exploitation Steps:**
1. Enumerating the terminal I found the following challenge files under `/ctf` and something of interest within the `~/.ash_history`:
   ![img](../images/REV-1.jpg)

2. Investigating further I found the `/mnt/shared/` directory contained the following:
   ![img](../images/REV-1a.jpg)

3. Looking at the `iv.bin` and `key.bin` files we can get the plain text strings:
   - Key: `1234567890abcdef`
   - IV: `0123456789abcdef`\

   _These appear to be directly used as encryption parameters for the AES operation._

4. The `encrypt.sh` script within the same directory shows exactly how the flag was encrypted from a file called flag.txt which was removed.
   
   ![img](../images/REV-1c.jpg)

   The script snippet below uses the `openssl enc` utility to apply AES-128-CBC encryption with the key and IV:
   ```bash
   openssl enc -aes-128-cbc -in flag.txt -out flag.enc \
      -K $(xxd -p key.bin) \
      -iv $(xxd -p iv.bin)
   ```   
   _This gave us a reliable blueprint to reverse the encryption process._

5. Using the recovered key and IV, we can manually decrypt the `flag.enc` file.

   ```bash
   openssl enc -d -aes-128-cbc -K 30313233343536373839616263646566 -iv 31323334353637383930616263646566 -in flag.enc
   ```
   **Note:** The key and IV must be converted from ASCII (plaintext) to hexadecimal before being passed to OpenSSL.

6. The command will successfully decrypted the file and revealed the plaintext flag.
   ![img](../images/REV-1b.jpg)