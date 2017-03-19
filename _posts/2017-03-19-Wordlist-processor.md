---
layout: page 
title :  Wordlist Prosessor
categories : [tools, password cracking]
---

Happy new year.  ( yeah.i know i been busy )

The last year end up been a cool year with a great end, i was speaker at two events with something i been working on for while now **password cracking**. 

Every thing started some time ago after googling around about password cracking, that i first shared  with my colleagues after a small  poc, and since to me everything less then 100% isn't worth spending time doing, i "went all in", end up to cracking every single hash. 

I been trying to be more active in the community and share something, whell this year the guys from **BSides Lisbon 2016** were kind enough to give me a shot in the **Lightning Talk**, and to my own astonishment i won the round and end up meeting some preaty cool people, so i guess i will see you guys their next year :). 
 
<iframe width="560" height="315" src="//www.youtube.com/embed/ISjXpYfnnMw" frameborder="0" allowfullscreen></iframe>

Five minutes fly, and i guess the topic was interesting enough, so i got an invitation to a small event that happends once a month, with **infosec** people, for the full version of the topic.  
The video is in portuguese, if you don't undersand it you can allways try to translate or invite me ;).  

<iframe width="560" height="315" src="//www.youtube.com/embed/UXpfqMgXbUw" frameborder="0" allowfullscreen></iframe>

## Wordlists every where 

It's true wordlists are everywhere, the people who share them say, they are sorted and efective, but if your used to use them you also noticed that some of them are preaty messed, **html tags, spaces, tabs empty lines** you can find preaty much any thing in their.  

One of the things i wasn't able to share with the guys at the time was some of my secret sauce, as usual statrs with a collection of **vim** macros and evolved to random scripts that i was at the time working on to integrate into something decent to share.  

### Wordlist Processor - wlpc

It's a small tool to clean maintain and normalize wordlists, the main objective is to be able to remove what is not usefull like **html tags, spaces, tabs empty lines** sort and remove duplicates but also say how much as removed and even what was removed.  

Wordlists are frequently at the heart of the password cracking task, its scary to even think that one of your **wordlist fu** actually removed a password you needed, and this was the main reason why i end up building a tool.  

{% highlight bash %}
[*]	Wellcome to wordlist processor

usage: main.py [-h] [-c] [-t] [-d] [-s] input output

positional arguments:
  input             Path to the wordlist to clean
  output            Path to the cleaned wordlist

optional arguments:
  -h, --help        show this help message and exit
  -c, --clean       clean Tabs and spaces
  -t, --tags        clean Tags xml and html
  -d, --duplicates  clean dulicates. Depends on sorting
  -s, --sort        Sorts the wordlist
{% endhighlight %}

The project is built in **python**  for now the performance is within the acceptable range, and is ready to handle gigabyte size wordlists. I'm not a fan of scripting tools as they are usually slow, but scripting is nice to build small **poc's** and the performance results usualy tell me if it would be best to be implemented in plain old and allways performant **C**. 

The output of the tool looks somthing like the follwing:

{% highlight bash %}
[*]     Wellcome to wordlist processor

        [*]     Source Wordlist:         leak2.lst
        [*]     Destination Wordlist:    out_leak2.lst
[*]     The folling actions will be executed:

        [*]     Trim tabs and spaces: True
        [*]     Remove Html Tags: True
        [*]     Sorts the wordlist: True
        [*]     Remove Duplicates: True
[*]     Results Status:

[-]     Do you wish to Continue? [y/N]y
[*]     This might take a while
[*]     Clean Temp file  clean_leak2.lst
[*]     Using temporary directorys
/tmp/11bn07dj46x2ty1vh05p7mxh0000gn/T
[*]     Finished sorting chunks
[*]     Merging chunks
[*]     Closing temporary file
/tmp/11bn07dj46x2ty1vh05p7mxh0000gn/T/000000
[*]     Removing tmp file clean_leak2.lst
[*]     Number of spaces removed 16
[*]     Number of html tags removed 6
[*]     Number of duplicates removed 71
{% endhighlight %}

It minght look strange at first to print all that information, but if you are bit like i'm you'r allways wondering what is been created, where and was it removed after its done. 
That is the main and only reason why the you seen the list of the temporary files and paths in the screen.

The project is can be found at my [github](https://github.com/0x4E0x650x6F/wordlist_processor), and as always happy hunting :).

Spetial thanks to every one from **BSides Lisbon 2016** and **AP2SI**. 