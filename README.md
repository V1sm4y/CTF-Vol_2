# CTF-Vol_2
CTF volume 2 on tryhackme , a great way to test your web testing skills

### 🥚 Easter Egg #1  
**📍 Location:** `http://10.10.194.27/robots.txt`

**🗭 Discovery Steps:**
- Accessed the `/robots.txt` file.
- Found a suspicious `Disallow:` path containing a long Base64 string.
- Also found a hex-encoded string at the bottom of the file.

**🔍 Method:**
- Decoded the hex string:  
  `45 61 73 74 65 72 20 31 3a 20 54 48 4d 7b 34 75 37 30 62 30 37 5f 72 30 6c 6c 5f 30 75 37 7d`
- Result: `Easter 1: THM{4u70b07_r0ll_0u7}`

**🏋️ Flag:**  
`THM{4u70b07_r0ll_0u7}`

**🧠 Notes:**  
- File had a troll comment suggesting the user-agent line might be bait.
- Multiple encoding layers detected in the disallowed path (Base64 → URL → Base64).
- Initial hex string gave the first flag directly.

