---
layout: page 
title: Slackware Minimal System
category: blog
---

The objective of this post is to give a jump start, and establish a working base, although this is a Minimal system created to address a production systems needs, there are things that are left out, so its not production ready.
Some of the security packages are not installed or not fully functional, that is a challenge i leave up to you. ;)  
Most of the information required to complete this task can be found in the slackware wiki, the process requires one to get familiar with dependency system. 

So "Practice makes it perfect!"
 
One of the ways to get to know this without having to try to run the application over and over again, is to use some BASH Foo, ldd can be used to track the dependencies, but this is a topic on its own. We will return to in other post, this is actually the main difference between the **LAME distros** like **Ubuntu**, and others like it that handle dependencies automatically, although this is quite useful hides the true nature of linux system, and leaves you out of many important things in any IT task "Practice, knowledge".  

This type of installations are used in testing and production environments and does not contain any development tools, so you will need 2 hosts, one for packaging, and the target system itself.
 
1. [Minimal System](http://www.slackwiki.com/Minimal_System#How_to_install_this_minimal_system "Slackwiki")

Now that we have the obvious out of the way, we might ask some interesting questions: 

**Q**: Why small?  
**R**: I can use two types of answers one, is the fact that security wise any package in a production system can be used against the system it self.   
The other is that a production system needs to have a role when that role is defined you will exclude things that will be around taking space memory and cpu time for nothing.  

**Q**: Do i have to choose all packages 1 by 1 ?  
**R**: No, if you don't know about tag files, by the end of this post you will love them.  

##Tag files:

Tag files are text files in the slackware install media they are used to assist the setup scripts what to install.
The Slackware dvd packages are organized by type: **a,ap,d,e,f,k,kde,kdei,l,n,t,tcl,x,xap,xfce,y**.
A minimal system requires the following package types : **a,ap,d,f,k,l,n**, and will end with a 490MB. 
 
2. [get the tagfiles]({{ site.url }}/assets/tagfiles.tar.gz)

##Tag file Format:
packagename: ACTION (SKP - SKIP;OPT - Optional;SKP - skip; ADD - Install)

<iframe width="560" height="315" src="//www.youtube.com/embed/kexpMNH2hvE" frameborder="0" allowfullscreen></iframe>
