---
title: TryHackMe - IDOR
author: rene
date: 2025-02-12 18:44:44 +0100
description: 🔑 Learn how to find and exploit IDOR vulnerabilities in a web application #
categories: [TryHackMe]
tags: [web]     # TAG names should always be lowercase
render_with_liquid: false
image:
    path: /images/tryhackme_idor/room_image.png
---
Learn how to find and exploit IDOR vulnerabilities in a web application giving you access to data that you shouldn't have.
[![Tryhackme Room Link](/images/tryhackme_idor/room_card.png){: width="300" height="300" .shadow}](https://tryhackme.com/room/idor){: .center }


## What is an IDOR?


IDOR stands for Insecure Direct Object Reference and is a type of access control vulnerability

This type of vulnerability can occur when a web server receives user-supplied input to retrieve objects (files, data, documents), too much trust has been placed on the input data and it is not validated on the server-side to confirm the requested object belongs to the user requesting it.

## An IDOR Example
Imagine you've just signed up for an online service, and you want to change your profile information. The link you click on goes to `http://online-service.thm/profile?user_id=1305`, and you can see your information.

Curiosity gets the better of you, and you try changing the user_id value to 1000 instead `(http://online-service.thm/profile?user_id=1000)`, and to your surprise, you can now see another user's information. You've now discovered an IDOR vulnerability! 

Ideally, there should be a check on the website to confirm that the user information belongs to the user logged requesting it.

Using what you've learnt above, click on the `View Site` button and try and receive a flag by discovering and exploiting an IDOR vulnerability.

We can see the 4 e-mails

Let's find out which one we can use this vulnerability for:

![IDOR Example](/images/tryhackme_idor/example.png){: width="600" height="450" .shadow}


Try changing the order number to 1000:

![IDOR Example 2](/images/tryhackme_idor/example2.png){: width="600" height="450" .shadow}

And we receive the flag:

![IDOR Example 3](/images/tryhackme_idor/example3.png){: width="600" height="450" .shadow}

Question:

What is the Flag from the IDOR example website?

##### Answer:
```
THM{IDOR-VULN-FOUND}
```

## Finding IDORs in Encoded IDs

### Encoded IDs

When passing data from page to page either by post data, query strings, or cookies, web developers will often first take the raw data and encode it. Encoding ensures that the receiving web server will be able to understand the contents. Encoding changes binary data into an ASCII string commonly using the a-z, A-Z, 0-9 and = character for padding. The most common encoding technique on the web is base64 encoding and can usually be pretty easy to spot. You can use websites like [base64encode](https://www.base64encode.org/) to decode the string, then edit the data and re-encode it again using [base64encode](https://www.base64encode.org/) and then resubmit the web request to see if there is a change in the response.


##### Answer:
```
base64
```

## Finding IDORs in Hashed IDs

### Hashed IDs

Hashed IDs are a little bit more complicated to deal with than encoded ones, but they may follow a predictable pattern, such as being the hashed version of the integer value. For example, the Id number `123` would become `202cb962ac59075b964b07152d234b70` if md5 hashing were in use.

It's worthwhile putting any discovered hashes through a web service such as [crackstation](https://crackstation.net/) (which has a database of billions of hash to value results) to see if we can find any matches.


##### Answer:
```
base64
```

## Finding IDORs in Unpredicatable IDs

### Unpredicatable IDs

If the Id cannot be detected using the above methods, an excellent method of IDOR detection is to create two accounts and swap the Id numbers between them. If you can view the other users' content using their Id number while still being logged in with a different account (or not logged in at all), you've found a valid IDOR vulnerability.



##### Answer:
```
2
```

## Where are they located?

The vulnerable endpoint you're targeting may not always be something you see in the address bar. It could be content your browser loads in via an AJAX request or something that you find referenced in a JavaScript file. 

Sometimes endpoints could have an unreferenced parameter that may have been of some use during development and got pushed to production. For example, you may notice a call to `/user/details` displaying your user information (authenticated through your session). But through an attack known as parameter mining, you discover a parameter called user_id that you can use to display other users' information, for example, `/user/details?user_id=123`.

If the Id cannot be detected using the above methods, an excellent method of IDOR detection is to create two accounts and swap the Id numbers between them. If you can view the other users' content using their Id number while still being logged in with a different account (or not logged in at all), you've found a valid IDOR vulnerability.

##### Answer:
```
No answer needed
```

## A Practical IDOR Example

Begin by pressing the Start Machine button; once started, visit the website in a new browser tab:

`https://WEBSITE_IP.p.thmlabs.com`

Firstly you'll need to log in. To do this, click on the customer's section and create an account. Once logged in, click on the Your Account tab. 

The Your Account section gives you the ability to change your information such as username, email address and password. You'll notice the username and email fields pre-filled in with your information.  

![Practice 1](/images/tryhackme_idor/practice1.png){: width="800" height="550" .shadow}

We'll start by investigating how this information gets pre-filled. If you open your browser developer tools, select the network tab and then refresh the page, you'll see a call to an endpoint with the path `/api/v1/customer?id={user_id}`

This page returns in JSON format your user id, username and email address. We can see from the path that the user information shown is taken from the query string's id parameter.

Let's try to update our username to something else and click update:

![Practice 2](/images/tryhackme_idor/practice2.png){: width="800" height="550" .shadow}

We can do a right click and select Edit and Resend:

![Practice 3](/images/tryhackme_idor/practice3.png){: width="800" height="550" .shadow}

Change the ID to `1` and click `Send`

![Practice 4](/images/tryhackme_idor/practice4.png){: width="800" height="550" .shadow}

We can see that ID has now changed and we can see the user:

![Practice 5](/images/tryhackme_idor/practice5.png){: width="800" height="550" .shadow}


##### Answer:
```
adam84
```

Now we do the same for the ID 3:

![Practice 6](/images/tryhackme_idor/practice6.png){: width="800" height="550" .shadow}

##### Answer:
```
j@fakemail.thm
```


---

<style>
.center img {
  display:block;
  margin-left:auto;
  margin-right:auto;
}
</style>