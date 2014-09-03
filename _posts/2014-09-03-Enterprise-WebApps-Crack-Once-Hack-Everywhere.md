---
layout: page 
title: Enterprise Web Applications, Crack Once Hack Everywhere
category: blog
---

Long time since i posted, Vulnerability hunting have kept me busy. FUN FUN :).  
Enterprise Web applications are old news, they are everywere. 
In this post i will talk about what seems to escape everyone, well not everyone...  

##Web Applications
One of the main reasons for the success of the web applications, is due to how easy they are to deploy.  
Fat client free workstations, allowed the illusion of simplicity, lower costs, and safety. but they were so wrong.   

##Lower Costs
This is probably the biggest Con ever sold! in IT.  

###Datacenter Before Web Applications.  

1. Domain.
2. Dns. 
3. Mail.
4. DMZ in case of a self hosted Website. 
5. Intranet web servers. (wikis,web etc.)
6. Databases.

The costs would be around the maintenance of all the workstations and keeping the applications updated.  
That's heavy work, and why maintenance and support teams where big.  
But the OS updates would force them to update, or deal with some painful incompatibilities (This made things safer!).  

###Datacenter, After Web Applications.

1. Domain.
2. Dns. 
3. Mail.
4. DMZ in case of a self hosted Website. 
5. Intranet web servers. 
6. Database Cluster (Web applications databases).
7. Serveral Web servers. (Clustering is common).

Yes thats correct, they have more servers, and more people by now they also need a team of Database Administrators, now the team actually got bigger,
and most of the maintenance is done on the application server.  
This means that you no longer need to install applications on the workstations.  
The applications updates lost priority, and since Web Applications struggle with Browser differences, most of them come with a "recommended Browser".  
That is a synonym for (Low quality work), basically how skilled the development team actually is.  
I usually calculate this by the number of browsers supported! (the lower the number lower the skill), single browser means amateur work, usually the kind you will likely receive when working with consulting company. 

###This is where the fun begins! 

See most of does companyes are only interested on selling their consultants not their work! reason why their work force is usualy full of junior developers. 

##Web Applications and Web Developers

Their skills are very diferent, although most of the people seem to ignore.  
A web developer is usual familiar with HTTP, I mean familiar because most of them seem to ignore the fact that you don't need a browser to call anything listening with HTTP!
(yes this have happed!)....  
Here's a list of the things that they're not used to deal with!.  

1. **Encryption**:  9-10, They will tell you that its safe to use MD5 or SHA-1. (Not true anymore sorry!).
2. **Concurrency**: This is a scary thing! most of them will run away as soon as you say the word... 
3. **Random**: Hot topic, most the then don't even know how random numbers are actualy generated! (but they use them....)
4. **C** Yes plain old **C** i realy think that you can't Call your self a Developer unless you know the machine, you can call your self (X language) Developer but not a developer...
5. **System Calls**: I could say that most of them don't know what a system call is! and it is true most of the cases but not realy needed for the work so.... 

The lack of knowledge is easly seen in the amount of vulnerabilities in the applications.  
Theirs nothing wrong in making mistates everyone does, the problem is the fact that they don't go away that they are overpriced.  

If you compare them with work done in C for example where you need a lot of knowledge to be productive, you see that vulnerabilities like Overflows are now almost extinct,
not just because apis got better but because the people that work with does languages had to get used to the idea of having to know some things before...  

Now look at the Web Development Sql, Cross site scripting, Cross site request forgery they are all old news, and they don't go away why is that?   
The last one is a clear evidence of their lack of knowledge on HTTP, the protocol that they all have to use.  

Products are now sold in many ways, as a service, installed locally, but most of them WEB, and everyone likes to have their logo on the page looks cool, and is also good for business!  
Some one once told me this while back when I was a junior developer "for a hammer everything looks like a nail", this is how they see enterprise needs.  

The story goes something like this!  

**Q**:Do you know javascript?  
**R**:Congrats, your now known a Web Developer.  

**Q**:Now what does this have to do security?  
**R**:Stay with me, i saved the best for last.   

##So what's wrong!

In a way Web applications had some advantages, the need to do more in less time lead to new programming languages, (so called safe! programming languages),
if that would be true all security companies would be out of business.  
This is a good thing for profit not a good thing for security, most of this languages are going towards simplicity more work in less time, the security thing they throw back at us
it's just collateral damage in order to simplify they remove all low lever stuff, that alone can make things pretty simple.

With less requirements their allowed to be productive faster, with less knowledge and that's where things get critical.  
I lost count of how many performance problems i was called to help, where the first thing I find were Files been opened and not closed, shore this wasn't the problem but
wasn't helping.  
When faced with the fact most of them replied:  
*"What the Garbage collector does not close the files! that's stupid!"*.  
Or my favourite, *"Thats the operating systems job!".* 

A they wonder why Concurrency is hard!....

**Q**:Who will want to hack "...."?  
**R**:This is a intranet WebApplication only internal people have access.  

This are only some of the most ignorant questions, I had to answer and they usually asked by the ones can't distinguish a computer from a brick.  
I'm talking about anything that have specific versions or fixed browser requirements usually used by Corp, or only works on some old technology that is either **DEAD**, or **OUTDATED**.  
(Yes including cobol!) Banks actually run this crap, in something that can't even use TCP\IP or is able to establish a Secure connection, you can forget about the tales of uber safe mainframes, because 9-10 they are communicating in **PLAIN TEXT**.  
I will leave to your imagination on how they make remote authentication!....  
From what i've seen, I bet you can't imagine how bad it really is, but its internal right so its safe (So they say!).  

Other joke of software is Siebel, that only work on Internet Explorer 6 (Yes there is such thing, and yest they buy this CRAP!), a clear example of how this things work.   
But wait they have a new version, that work in around 4 browsers (don't pop the champagne yeet!) including Chome 2.x (no need check the post's date, that version is outdated).  
Their new product is locked on a outdated version of a browser, with automatic updates! (PURE SKILL...).   
In this case they even got in to an argument with M$, because it was said that in order to be able receve a set updates the browser update would be mandatory, and so they changed it.  

But this are not alone.  

1. Banking (internet, and intranet).
3. Flight companies.
4. Internal WebApplication.
5. Services running old versions of java, .net, PHP, Python.
6. Online Shops (Books,Traveling,etc).

They are all behind the ilusion, the idea that internal systems are safe because their are not exposed to the internet.  
Well as usual there wrong.

##Users
All comes down to their users, they need the internet to do their work among other more personal things (the ones that can).  
If you're not convinced ask your selfs this.

**Q**: How many users will change browser to do their personal "thing"?  
**Q**: How many users will change browser to do work search ?  

**R**: NONE, that's right unless the page gets unsuable.   
They wont they don't need to its not their job, and most of the time they don't even have the skills to determine what
might happen (else they would be using windows in the first place).  

In short, all resumes to this!  

**Q**: Are they saving money?  
**R**: I really don't think so...  
**Q**: Are their systems more secure ?  
**R**: No, now all they need is to hack the users browser, and since it might be locked in a version they can focus a target.  

I will end this post with this:    
In the today's world, in order to hack in to a Enterprise, all you have to do is hack their browser, after all their are all **INTERNAL WEB APPLICATIONS**!  

![Internet Explorer Pwned!]({{ site.url }}/assets/ie6-pwn.png)






