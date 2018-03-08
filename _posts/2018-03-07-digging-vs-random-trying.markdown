

---
title: Digging into the Root Cause VS. Random Trying. 
layout: post
tags: life
comments: no
---

Digging into the root cause of a problem is better than random trying. 

For example, today I spent a couple of hours on how to properly set the logging configuration for ZooKeeper 3.5.3-beta. This is what I did. 

1. After starting ZooKeeper server, I found a zookeeper-xxx.out file under ./logs/ and thought it was the log file. There’s some INFO level log in it. I found a log4j.properties file under ./conf/ and tried modify it to output DEBUG level log. It didn’t work. I thought it’s because I made mistakes configuring log4j, and spent perhaps an hour modifying log4j.properties in different ways until I gave up. 
2. I started searching on google “zookeeper.log not created” because I saw in log4j.properties that the correct log file for ZooKeeper is zookeeper-xxx.log (not zookeeper-xxx.out). This is the first correct decision I made - question from the beginning : what is the zookeeper log file and do I really have it? Turns out I didn’t. 
3. A bunch of answers popped up on google. I spent about another trying each one of them (restarting zookeeper, adapting the answer to my scenario, etc.). Eventually one of them worked. The solution is to create a zookeeper-env.sh file and set the environment variable ZOO_LOG4J_PROP properly in it. I started looking at why? Turns out log4j.properties is used but always gets overwritten by zkEnv.sh which sets ZOO_LOG4J_PROP=INFO. So modifying log4j.properties is meaningless. But when you have a zookeeper-env.sh in ./conf/ directory, zkEnv.sh will use the configuration you write in zookeeper-env.sh instead of its own. So a more direct solution is to modify zkEnv.sh and set ZOO_LOG4J_PROP=DEBUG,ROLLINGFILE directly. 

Instead of spending more than 2 hours on trying random answers from google, I could have done this. 

1. Read ZooKeeper official documentation about logging, and notice that 1) modifying log4j.properties doesn’t have any influence, 2) I don’t have the log file zookeeper-xxx.log while running ZooKeeper. 
2. Read log4j official document, and learn that the way to use log4j is to write log4j.properties. However, notice that log4j.properties is only setting system properties, and those system properties can be modified by setting -Dxxx=XXX while executing java command. 
3. Look into zkServer.sh which is the script used to start ZooKeeper, you can find the real java command to start ZooKeeper, and it does take -Dzookeeper.root.logger=XXX as one of its arguments. 
4. Read zkServer.sh to find where this -Dzookeeper.root.logger is coming from, you’ll see it’s set in zkEnv.sh. Problem solved. Change it in zkEnv.sh. 

The 2nd method is better in that 1) it doesn’t waste time on random trying solutions, 2) if you can really understand all the logic details, after fixing it, you’ll learn about zookeeper, log4j, shell script, java system properties, etc. This method is “reasoning”. 

The 1st method’s advantage is that there is a chance that the first answer you tries solves the problem and saves your time. If you lack a lot of knowledge, it could take you forever to learn about everything and perform the second method. 

Another disadvantage of the 1st method is you might miss some answers since there are so many on the internet. In this case, I missed one stackoverflow answer in my first step - which actually is the correct answer and could have saved me 2 hours. (StackOverflow is good.)