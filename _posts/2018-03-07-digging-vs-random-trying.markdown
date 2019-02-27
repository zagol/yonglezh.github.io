---
title: Digging into the Root Cause VS. Random Trying. 
layout: post
tags: life
comments: no
---

_This post is an example which shows that a good article requires endless refinement no matter how good you feel about it in your early passes. 
I only did 1 pass for this article. Because in my first try I felt that I was in the zone and wrote down everything perfectly. The next day when I read it, I didn't know what I was thinking -- the points I wanted to make spread across the article without a clear logic flow connecting them. This post is kept here as a reminder._

Digging into the root cause of a problem is better than random trying solutions. 

## Real life example

Today I spent a couple of hours on how to properly set the logging configuration for ZooKeeper 3.5.3-beta. This is what I did. 

1. Wasting time because of a stupid assumption.

    After starting ZooKeeper server, I found a zookeeper-xxx.out file under ./logs/ and assumed it was the log file. There’s some INFO level log in it. I found a log4j.properties file under ./conf/ and tried modify it to make Zookeeper output DEBUG level log. It didn’t work. I thought it’s because I made mistakes configuring log4j, and spent perhaps an hour learning & modifying log4j.properties in different ways until I gave up. 

2. Realized zookeeper-xxx.log is the log file instead of zookeeper.out.

    I saw in log4j.properties that the correct log file for ZooKeeper is zookeeper-xxx.log (after ignoring it for over an hour). I started searching on google “zookeeper.log not created”. 

    This is the first correct decision I made, i.e., **question & try to understand it in an operational manner instead of guessing & making assumptions easily** - what is the Zookeeper log file and do I really have it? Turns out that I didn’t. 

3. Random trying each solution popped up on Google. 

    A bunch of answers popped up on google. I spent about another hour trying each one of them. A lot of time was spent on restarting Zookeeper, adapting the answer to my scenario, etc. **The worst part is that when a solution doesn't work I'm not sure whether it's because it's not the correct solution or it's because I need to somehow adapt it to my environment.** 

    Eventually one of them worked. The solution is to create a zookeeper-env.sh file and set the environment variable ZOO_LOG4J_PROP properly in it. **A bigger problem of this random-trying methodology is that I don't know why the solution worked.**

    I started looking at why the solution worked. Turns out log4j.properties is used but always gets overwritten by zkEnv.sh which sets ZOO_LOG4J_PROP=INFO (luckily Zookeeper developers realized the problem and changed their way in the released version). So modifying log4j.properties is pointless. But when you have a zookeeper-env.sh in ./conf/ directory, zkEnv.sh will use the configuration you write in zookeeper-env.sh instead of its own. So a more direct solution is to modify zkEnv.sh and set ZOO_LOG4J_PROP=DEBUG,ROLLINGFILE directly. 

Instead of spending more than 2 hours on trying random answers from google, I could have done this. 

1. Read ZooKeeper official documentation about logging, and notice that 1) I don’t have the log file zookeeper-xxx.log while running ZooKeeper, 2) (optional) modifying log4j.properties doesn’t have any influence. 

2. Read log4j official document, and learn that the way to use log4j is to write log4j.properties. Moreover, also learn that log4j.properties is only setting system properties, and those system properties can be modified by setting -Dxxx=XXX while executing java command. 

3. Look into zkServer.sh which is the script used to start ZooKeeper, you can find the real java command to start ZooKeeper, and it does take -Dzookeeper.root.logger=XXX as one of its arguments. 

4. Read zkServer.sh to find where this -Dzookeeper.root.logger is coming from, you’ll see it’s set in zkEnv.sh. **Now you understand the problem**. Problem solved - change it in zkEnv.sh. 

**This methodology is reasoning - you try to understand the problem before designing a solution according to your understanding of the root cause.** 

Computer science is a great field to apply this methodology. Because computer science might be the only subject that we human beings have the **ground truth** - we know how a computer runs step by step. It means we don't have to have hypotheses about a problem, we should be able to understand how it happens step by step. (In other fields such as physics, people have to agree on some basic first principles and reason from there. Moreover, from time to time people run into contradiction reasoned from those principles and have to change them.)

## Advantage of the second methodology 

1. You won't waste time on the "random trying" rabbit hole. 

    ```
    Random trying solutions, 
    -> if no one worked, for each of the solutions that don't work, guess and random try new solutions/adjustments
    -> if no one worked, repeat ... 
    ```
    
    If you are lucky, you'll get a solution that works somewhere in this infinite expanding tree.

2. You'll have a clear picture of how everything works together in an operational way. After fixing this problem, you’ll learn about Zookeeper, log4j, shell script, java system properties, and how they work together. 


The "random trying" methodology’s advantage is that there is a chance that the first answer you tries actually solves the problem and saves your time. If you lack the basic knowledge of Linux, it could take you forever to learn about everything and perform the second method. 

Another disadvantage of the 1st method is you might miss some solutions since there are so many on the Internet. In this case, I missed one StackOverflow answer during my first step. It actually is the correct answer and could have saved me 2 hours. (StackOverflow is awesome.)

In real life, we can't afford to dig into the bottom of every problem. What's the proper balance between reasoning and guessing? I don't think there's a universal answer. However, I do believe that a person should reason as much as he/she can. 
