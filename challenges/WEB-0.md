## WEB-0: Session Analysis
**Description:**
This challenge involved analyzing a web application's session handling mechanism to retrieve a flag protected by cookie-based authentication.
<details> <summary><b>Reveal Flag</b></summary> flag{5b3110c1dd} </details></br>

**Solution Summary:**
- Identified that the flag endpoint required a valid session cookie.
- Retrieved a session token from the `/login` endpoint.
- Used the token to access the protected `/flag` route.

**Exploitation Steps:**
1. Explore the `/ctf` directory and review the source code:
   ```bash
   ls /ctf
   cat client.py
   cat server.py
    ```
2. Investigating the behaviour of the `server.py` script I identified that the `/flag` route checks the browser for a session cookie before serving the flag and further on the `/login` route looked to provide the needed cookie.
    ![screenshot](../images/WEB-0.jpg)

3. Using `wget` sent a request to /login to obtain a valid session:
    ```bash
    wget -S http://192.168.1.2/login
    ```
    Output included:
    ```plaintext
    Set-Cookie: session=NmUnt65k75dZxATkaycYpl88Y4hGShBS
    ```
4. Sent a follow-up request to `/flag` with the session cookie and upon viewing the downloaded flag file we have another flag.
    ```bash
    wget -S --header "Cookie: session=NmUnt65k75dZxATkaycYpl88Y4hGShBS" http://192.168.1.2/flag
    ```
    ![screenshot](../images/WEB-0a.jpg)

