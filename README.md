# CTF-Vol_2
CTF volume 2 on tryhackme , a great way to test your web testing skills
Alright, so here’s what I found first while poking around the machine.

I hit up `http://10.10.194.27/robots.txt` just to check if the devs were hiding anything juicy. Classic move, right? And boom—there it was:

```
User-agent: * (I don't think this is entirely true, DesKel just wanna to play himself)
Disallow: /VlNCcElFSWdTQ0JKSUVZZ1dTQm5JR1VnYVNCQ0lGUWdTU0JFSUVrZ1p5QldJR2tnUWlCNklFa2dSaUJuSUdjZ1RTQjVJRUlnVHlCSklFY2dkeUJuSUZjZ1V5QkJJSG9nU1NCRklHOGdaeUJpSUVNZ1FpQnJJRWtnUlNCWklHY2dUeUJUSUVJZ2NDQkpJRVlnYXlCbklGY2dReUJDSUU4Z1NTQkhJSGNnUFElM0QlM0Q=
```

At the very bottom of the file, I noticed something else too—just a string of hex values:

```
45 61 73 74 65 72 20 31 3a 20 54 48 4d 7b 34 75 37 30 62 30 37 5f 72 30 6c 6c 5f 30 75 37 7d
```

So I was like, okay, let’s see what this is hiding.

### Step 1: Checked that Base64 string in the `Disallow:` field
At first glance, that looked like Base64, so I threw it into a decoder. The first result looked like a URL-encoded string. Decoded that too. Then another round of Base64.

But honestly, the result still felt off. Looked more like garbage or some multi-layer obfuscation, so I shelved that for now and focused on the easier win.

### Step 2: That Hex Tho 
Now this hex string? Straightforward.
I popped it into a hex-to-text converter (you can even do it manually if you're wild like that) and boom:

```
Easter 1: THM{4u70b07_r0ll_0u7}
```

Easy first flag. Let’s gooo 🏁

### ✅ Flag:
`THM{4u70b07_r0ll_0u7}`

Moral of the story? Always check `robots.txt` — sometimes devs can’t help themselves and leave the good stuff out in the open. One egg down, nineteen to go. Let’s keep cookin’.

