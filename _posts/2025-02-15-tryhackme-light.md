---
title: TryHackMe - Light
author: rene
date: 2025-02-15 09:11:12 +0100
description: A quick overview of how to use SQL Injection 💉 to solve this challenge #
categories: [TryHackMe]
tags: [web, sqli]     # TAG names should always be lowercase
render_with_liquid: false
image:
    path: /images/tryhackme_light/room_image.png
---

### Intro
This is one of the easier challenges as it's marked *Easy*. We are expected to use some kind of **SQL Injection** vulnerability.


[![Tryhackme Room Link](/images/tryhackme_light/room_card.png){: width="300" height="300" .shadow}](https://tryhackme.com/room/lightroom){: .center }

Welcome to the Light database application!

I am working on a database application called Light! Would you like to try it out?
If so, the application is running on `port 1337`. You can connect to it using `nc 10.10.253.14 1337`
You can use the username smokey in order to get started.


### Nmap

Nmap scan is not necessary as the room tells us to connect to port 1337 and also a user to start with.

```
nc 10.10.253.14 1337

user: smokey
```

![Flag](/images/tryhackme_light/connect.png){: width="600" height="450" .shadow}


*Since this is a database challenge, let's use some basic SQL Injection*
```
'
```
### SQLi

![Flag](/images/tryhackme_light/sql1.png){: width="600" height="450" .shadow}

Maybe SELECT?


![Flag](/images/tryhackme_light/sql2.png){: width="600" height="450" .shadow}

Seems like there is some sort of a word blacklist.

Blacklist seems to be *case sensitive*.


![Flag](/images/tryhackme_light/sql3.png){: width="600" height="450" .shadow}

Seems like we've got something:

![Flag](/images/tryhackme_light/sql4.png){: width="600" height="450" .shadow}

Let's list the users  and their passwords of `usertable`:

![Flag](/images/tryhackme_light/sql5.png){: width="600" height="450" .shadow}


Nothing suspicious, let's list the `admintable`:

### Flag

And we receive our flag:

![Flag](/images/tryhackme_light/sql6.png){: width="600" height="450" .shadow}



---

<style>
.center img {
  display:block;
  margin-left:auto;
  margin-right:auto;
}
</style>