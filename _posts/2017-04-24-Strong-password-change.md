---
layout: page 
title : Strong Password change
categories : [security, password]
---

Hey what's up, yeah i know its been a long time since i posted something.

## Strong password change

Over the last years alot of things have been said about strong passwords and the meaning of a **strong password**, as the **CPU** power increases the **number and range of charecters** also increase. This makes things hard at many levels, from the **users** prespective think about a string that meets all the requesits and remember it, it's only the first step, lately some have enven sugested that passwords in enviroments such as Local domains (Active Directory, OpenLDAP, etc), shouln't even expire, this to me sounds crasy but i will not try to explain my point of vew instead i will use one of arguments wich i find reasonable valid and propose something that could help one to prevent users from abusing the system and create weak passwords


### The increment a digit trick

This is probably one of the most konwn trick, works in every system i have found and been able to test, and is a valid arguement in the **password change & secure password** discution.

In almost every system, a user is initialy invited to set an initial password that meets a set of requirements like: **lower and upper case digits and a simbol** with a given **minimum length**, after a while the same user is invited to change that passsword, some time before it expires, and this is where the discution beguins.

What many say is **True** most users will take the old password and increment a digit or change the symbol, essentialy the structure of the password remainds the same.

One of the first questions one might ask is, Why is this a problem in a domain ?
Well, if one consider the fact that most **local domains** will lock the user after 3 failed attempts, if the user relies on this trick its reasonable to assume that a malicious user will guess the **new password** before the user gets locked.


### A Possible solution

In one of my last security conferences where i was a speaker, some one maded a question with a similar issue, meaning how to detect weak passwords, or password changes such as the one i described previously. One of methods used is the wordlist, this is an effective method unfortunatly this list will grow as IT and **CPU power** increases, reason why i don't consider this as a reliable solution.


One of my favorite password cracking methods are the masks, wich efectify reduces the number of permutations and allows one to optimize the mask to something closer to what a user might chose in orther to remember. In this method a charecter range is setup using mask where every position in the password would be translated to a symbol:

1. **BahBeh**: UllUll
2. **BahBeh123$**: ULLULLDDDS

Given this example is easy to understand what means what, charters are organyzed by sets (Uppercase, Lowercase, Numbers, Symbols), when comparing to password masks becomes easy to verify if they are using the same mask, and prevent the user from using the same tick.


The Password change process usualy implemented as follows:

1. Ask the user for the **old password** and the **new password**.
2. Hash the **old password**
3. Check if the **old password hash** is correct.
4. Check password complexity.
5. Hash the **new password**.
6. Save the **new password hash**

Note that the **old password** can be found in plain, this allows one to translate the password to a mask force the user to change the mask of the password.

In short the new password change process would become.

1. Ask the user for the **old password** and the **new password**.
2. Hash the **old password**
3. Check if the **old password hash** is correct.
4. Translate the **old password** to mask.
5. Translate the **new password** to mask.
6. Check **new password** complexity.
7. Compare **old password mask** and the **new password mask**.
8. Hash the **new password**.
9. Save the **new password hash**
 
Sounds easy right ? if one takes a closer look will become clear that this is allows other checks, and render's the weak password wordlist irelevant.

For does who like to see the code here is sample implementation in python.

{% highlight python %}
#!/usr/bin/env python
# encoding: utf-8
# -*- coding: utf-8 -*-
#
# A poc on how to one could implement a "strong"
# Password change and prevent weak pwd changes 
# souch as increment 1 digit
# Created by 0x4E0x650x6F on 24/07/2017.
# Copyright 2017 0x4E0x650x6F. All rights reserved.

import re
from hashlib import pbkdf2_hmac
from binascii import hexlify


class Maskify(object):
    """
        Convert Password to mask eg:
        blahblah123$ to LLLLLLLLDDDS
        L : lower case
        U : Upercase 
        S : Symbol spetial chars
    """
    def __init__(self, clregex=r"[a-z]", curegex=r"[A-Z]", nregex=r"[0-9]", 
            sregex=r"[±§!\"#$%&/(){}\[\]'?+*ºª\´\`^~;,:\.-_<>]"):
        self.clregex = clregex
        self.curegex = curegex
        self.nregex = nregex
        self.sregex = sregex
        self.clchar = "l"
        self.cuchar = "u"
        self.nchar = "d"
        self.schar = "s"

    def to_mask(self, pwd):
        mask = None
        #replace a-z lower case
        mask = re.sub(self.clregex, self.clchar, pwd, 0)
        # replace A-Z upper case
        mask = re.sub(self.curegex, self.cuchar, mask, 0)
        # replace numbers
        mask = re.sub(self.nregex, self.nchar, mask, 0)
        # replace symbols
        mask = re.sub(self.sregex, self.schar, mask, 0)
        return mask.upper()
        

class Authenticate(object):
    """
    Emulate Authentication and password change
    """
    ALGORITHM = "sha256"
    def __init__(self, pwd):
        self.hash = self.__hash(pwd)
        self.maskify = Maskify()
    
    def __hash(self, pwd):
        """
        This will not use salt as the objective 
        is to work as an example!
        """
        hash_key = pbkdf2_hmac(Authenticate.ALGORITHM, pwd, '', 100000)
        return hexlify(hash_key)
    
    def authenticate(self, pwd):
        """
            Given a pwd will 
            hash given :pwd and compare with instance pwd 
        """
        uhash = self.__hash(pwd)
        if uhash == self.hash:
            return True
        return False
    
    def check_complexity(self, old_mask, new_mask):
        """
        Returns: True if the pwd complexity is 
        suficient
        """
        if old_mask != new_mask:
            return True
        else:
            return False
        

    def change_pwd(self, old_pwd, new_pwd):
        # step 1: check if oldpwd is correct!
        if self.authenticate(oldpwd):
            old_mask = self.maskify.to_mask(old_pwd)
            new_mask = self.maskify.to_mask(new_pwd)
            # step 2: check complexity
            if self.check_complexity(old_mask, new_mask):
                # if true means the mask is NOT the same
                # their for is NOT a weak password
                return True
        # doe NOT matter where it fails...
        return False



if __name__ == "__main__":
    
    print "[*] Poc weak password change detector"
    
    oldpwd = "blahblah123$"
    npwd = "blahblah124$"
    npwd2 = "blah124$blah"
    
    auth = Authenticate(oldpwd)
    result_equal = auth.change_pwd(oldpwd, npwd)
    print "[*]\tTesting change with same mask %s" % result_equal

    result_not_equal = auth.change_pwd(oldpwd, npwd2)
    print "[*]\tTesting change with same mask %s" % result_not_equal
{% endhighlight %}


