---
title: Buzzwords 
layout: post
tags: life
comments: no
---

Goal: Avoid using buzzwords in this post. 

What is a buzzword? A word used without clear definition and context, unintended or intended to win the argument instead of making things clear. 

Example here. 

> Words that make us feel good just because we say them. 
> 
> From Buzzwords in _[The Big Questions - A Short Instroduction to Philosophy](http://elibrary.bsu.az/books_400/N_17.pdf)_

Really few words that human beings say can stand a close scrutiny if we demand an extremely clear definition. 

We only require a word to be clear within the context of its usage. 


Don't be scared off by buzzwords. 

Don't be scared off by people who talks like they know better but cannot even describe their concepts clearly and only uses buzzwords to show off. 

Functional programming was a buzzword for me for years. Only when I realized that you can do it in C using function pointers plus carefully passing the context plus careful avoiding side effect ([stackoverflow](https://stackoverflow.com/questions/1839965/dynamically-creating-functions-in-c)), it becomes clear to me that more important thing is the usage scenario: do you force to use it in your entire language? what's the best scenario to force using it? when does it have a clear advantage? 

For example, I always hear people say that in Lisp functions are treated the same as variables. The first thing came to my mind is that you can modify a function! Taking a closer look shows that you cannot, or at least cannot do it directly (you can define a macro that adds the function's source code into the symbol list - [stackoverflow](https://stackoverflow.com/questions/1220149/can-you-show-me-how-to-rewrite-functions-in-lisp)). 

Without clearly defining what's their advantage in concrete usage scenarios, all programming paradigms are just syntactic sugers. (Foundamentally all the turing-complete languages are equal. It means they only have syntactic sugers compared to each other. What is "syntactic suger" anyways? Am I using a buzzword here? Is "syntactic suger" just another word for abstraction? So all programming paradigms are different abstractions for sure, with its scenario clearly defined. They can be implemented in any turing-complete language without support for the programming paradigm.) 

> it all gets compiled down to 1's and 0's eventually
>
> I apologize if I am interpreting a light hearted comment as a serious statement, but the point of programming languages is to aid in getting the correct 1s and Os written.

This is a good point. Although you can definitely write the same thing using machine code and Rust, the ability Rust has to guarantee the correctness during the development is different. So different language paradigm (abstraction) is important for getting certain job done in certain context QUICKLY and CORRECTLY. Then the question becomes: when does functional programming (or any programming paradigm) help with quickness and correctness or maintainableness? 

What techniques really work? 
Internet. Crypocurrency (not in its designed way, but its ability to create unchangable records). 

What worked? 
Virtual memory*
Address spaces*
Packet nets*
Objects / subtypes
RDB and SQL
Transactions*
Bitmaps and GUIs*
Web
Algorithms

Not yet working? 
Capabilities*
Fancy type systems*
Functional programming
Formal methods*
Software engineering
RPC (except for Web)*
Distributed computing*
Persistent objects
Security*

Maybe working? 
Parallelism
RISC
Garbage collection
Interfaces and specifications
Reuse (Works for Unix filters
Big things (OS, DB, browser)
Flaky for Ole/COM)
