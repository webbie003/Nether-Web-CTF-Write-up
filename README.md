# Nether Web - Browser-Based CTF Write-Up

## Overview
While preparing for the CEH (Certified Ethical Hacker) exam, I came across the **Nether Web CTF** ([parath.net](https://parath.net/)) which was mentioned in the **r/securityCTF** community on Reddit. This browser-based CTF features eight progressive challenges—referred to as Missions—that test a range of web security concepts, including input validation, authentication bypass, insecure client-side controls, and session management flaws. I set out to complete all eight tasks, applying techniques learned during my CEH studies and using the experience to reinforce my practical skills through hands-on application.

**Difficulty:** Easy – Medium

## Challenge Description
Nether Web presents a series of missions spanning reconnaissance, analysis, cryptography, web exploitation, and system-level attacks. Each challenge is designed to emulate real-world vulnerabilities, encouraging participants to apply practical cybersecurity skills in a hands-on, progressive format.

**Challenge Series:** Nether Web  
**Format:** Browser-based, progressive web application vulnerabilities  
**Objective:** Exploit various weaknesses in a simulated web application to progress through each mission and ultimately capture all flags.

## Challenge Index
- [MISC-0: Information Gathering](challenges/MISC-0.md)
- [REV-0: Reverse Engineering](challenges/REV-0.md)
- [CRY-0: Cryptanalysis](challenges/CRY-0.md)
- [WEB-0: Session Analysis](challenges/WEB-0.md)
- [PWN-0: System Exploitation](challenges/PWN-0.md)
- [MISC-1: Trace Investigation](challenges/MISC-1.md)
- [REV-1: Flag Authentication](challenges/REV-1.md)
- [WEB-1: Command Injection](challenges/WEB-1.md)

## Tools Used
- GDB
- GCC
- Objdump
- Python3
- OpenSSL
- Netcat
- wget
- Core Linux utilities (e.g., cat, ls, grep, nano etc.)


## Reflection
Completing the Nether Web CTF provided a practical, hands-on opportunity to apply a range of web exploitation techniques in a safe and structured environment. It reinforced concepts I had studied for CEH while exposing me to real-world attack vectors involving insecure input handling, poor session control, and unsafe cryptographic implementations.

## Acknowledgments
Special thanks to [u/PrimaryAdventurous97](https://www.reddit.com/user/PrimaryAdventurous97/) for publishing the Nether Web CTF and sharing it with the community on [r/securityCTF](https://www.reddit.com/r/securityCTF/).
