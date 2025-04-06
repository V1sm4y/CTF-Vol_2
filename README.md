Alright, so here’s what I found first while poking around the machine.

First off, we hit it with the usual combo—Nmap and Gobuster. Just to open up the field and see what’s up.

### Step 0: Reconnaissance with Nmap

Here’s the command I ran:
```bash
sudo nmap -Pn -sS -sV -sC ctf.thm
```
Classic aggressive scan with service/version detection and default scripts. The scan took a while but here’s the output that stood out:

```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 5.9p1 Debian 5ubuntu1.10 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.2.22 ((Ubuntu))
```

HTTP and SSH—solid. Noted the Apache version and jotted it down for later in case there are known exploits.

Then I checked the scripts output for HTTP and saw something juicy:
```
| http-robots.txt: 1 disallowed entry 
|_/VlNCcElFSWdTQ0JKSUVZZ1dTQm5JR1VnYVNCQ0lGUWdTU0JFSUVrZ1p5QldJR2tnUWlCNklFa2dSaUJuSUdjZ1RTQjVJRUlnVHlCSklFY2dkeUJuSUZjZ1V5QkJJSG9nU1NCRklHOGdaeUJpSUVNZ1FpQnJJRWtnUlNCWklHY2dUeUJUSUVJZ2NDQkpJRVlnYXlCbklGY2dReUJDSUU4Z1NTQkhJSGNnUFElM0QlM0Q=
```
Yep. We’re gonna come back to that.

### Step 1: Gobuster Time
You already know we’re gonna comb that web server. So I let `gobuster` loose with the good ol' common.txt wordlist:
```bash
gobuster dir -u http://ctf.thm -w /usr/share/wordlists/dirb/common.txt
```
Here’s what popped up:
```
/button               (Status: 200)
/cat                  (Status: 200)
/index.php            (Status: 200)
/iphone               (Status: 200)
/login                (Status: 301) [--> http://ctf.thm/login/]
/robots.txt           (Status: 200)
/small                (Status: 200)
/static               (Status: 200)
```
We’re seeing a bunch of valid responses—`/button`, `/cat`, `/iphone`, `/small`, and of course `/robots.txt`.

So I hit up `http://10.10.194.27/robots.txt` just to check if the devs were hiding anything juicy. Classic move, right? And boom—there it was:

```
User-agent: * (I don't think this is entirely true, DesKel just wanna to play himself)
Disallow: /VlNCcElFSWdTQ0JKSUVZZ1dTQm5JR1VnYVNCQ0lGUWdTU0JFSUVrZ1p5QldJR2tnUWlCNklFa2dSaUJuSUdjZ1RTQjVJRUlnVHlCSklFY2dkeUJuSUZjZ1V5QkJJSG9nU1NCRklHOGdaeUJpSUVNZ1FpQnJJRWtnUlNCWklHY2dUeUJUSUVJZ2NDQkpJRVlnYXlCbklGY2dReUJDSUU4Z1NTQkhJSGNnUFElM0QlM0Q=
```

At the very bottom of the file, I noticed something else too—just a string of hex values:

```
45 61 73 74 65 72 20 31 3a 20 54 48 4d 7b 34 75 37 30 62 30 37 5f 72 30 6c 6c 5f 30 75 37 7d
```

So I was like, okay, let’s see what this is hiding.

### Step 2: Checked that Base64 string in the `Disallow:` field
At first glance, that looked like Base64, so I threw it into a decoder. The first result looked like a URL-encoded string. Decoded that too. Then another round of Base64.

But honestly, the result still felt off. Looked more like garbage or some multi-layer obfuscation, so I shelved that for now and focused on the easier win.

### Step 3: That Hex Tho 👀
Now this hex string? Straightforward.
I popped it into a hex-to-text converter (you can even do it manually if you're wild like that) and boom:

```
Easter 1: THM{4u70b07_r0ll_0u7}
```

Easy first flag. Let’s gooo 🏁

### ✅ Flag:
`THM{4u70b07_r0ll_0u7}`

Moral of the story? Always check `robots.txt` — sometimes devs can’t help themselves and leave the good stuff out in the open. One egg down, nineteen to go. Let’s keep cookin’.

