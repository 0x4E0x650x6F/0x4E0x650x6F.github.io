---
layout: page 
title :  Wordpress Admin Login Proctection Myths
categories : [security, web-authentication]
---

Some time last week, I found some posts on the web suggesting that one could protect the **"broken"** Wordpress  authentication with the weirdest ideas from **ip blocking plugins**, to **HTTP Basic Authentication** in front of the wp-login.php.  
  
Yeah I think they are funny to.  

In this post I will try explain why this is a **horrible idea**. 
Their are many ways to make the attacker's life difficult, one the most effective is, to make the **brute-force** as slow as possible or really difficult to **automate**.  

Unfortunately the wordpress login implementation is basically a joke, beginning with what **"seems to be a csrf or cookie test"**, a set of **static values** on hidden fields (testcookie) and in the cookies (wordpress_test_cookie), I never understood what's the point of adding static junk to the request, in my first video i explained how one could use hydra to search for weak credentials. 

## Vanilla Wordpress Brute-force
<iframe width="560" height="315" src="//www.youtube.com/embed/onwo77kb7Jw" frameborder="0" allowfullscreen></iframe>

I the process of recording this video, I noticed that wordpress used **different** error messages for **non existing user** or **incorrect password**, initially I thought this was all too bad to be true, and could be some development configuration sadly it's not, not only is easy to brute-force, but also easy to harvest valid usernames, so it's officially a mess!  

## Protecting a login with a login ! 

Web development is not rocket science, starting with the fact the weakest link is **allways** the person, behind the keyboard having two sets of credentials might be a **bad idea**, since most of the ppl use the same or similar passwords for everthing, rendering the dual autientication useless, even if one does the smart thing and choses to have two diferent sets of credentials, they have to be maintained and both of them can be brute-forced, this without mentioning the fact that **HTTP Basic Authentication** places the base64 encoded username:password on the **HTTP** header making it easy to intercept.  


To demostrate this I made other video, this time explaining how one could brute-force a login **"hidden"** behind authentication. 

<iframe width="560" height="315" src="//www.youtube.com/embed/h5fsRl_Fovg" frameborder="0" allowfullscreen></iframe>

###Moral of the story 

Don't use wordpress :) but if you really have to use it here are my suggestions: 

**Place a captcha on the login form**.  
**Place a proper CSRF token**.  
**Use same message for non existing username or incorect password**.  

This can make brute-force and username harvesting automation more difficult but not impossible ;).