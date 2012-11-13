---
layout: post
categories: 
    - javazone
    - comfortzone
    - hello
title: Get Started Performance Testing Using The Grinder
published: true
author: Espen Herseth Halvorsen & Kjetil Valle

---

**Performance testing is important. 
In this blog post we will explain why, introduce you to a tool for testing performance called The Grinder, and take you through five examples which will get you started using Grinder for testing your web application.**

The post is based on workshops held at [ROOTS](http://www.rootsconf.no/talks/42) and [FreeTest](http://free-test.org/node/81#2012051009-EspenHalvorsen), and the materials -- slides, tasks and code -- are of course [available on GitHub](https://github.com/kvalle/grinder-workshop).
To get the most out of the examples, you may want to follow along with the materials trying each task on your own, before reading the solutions we present here.


So, Why Should I Test Performance?
----------------------------------

Well, the fact that you are reading this post, and haven't fallen asleep already is a good sign – you obvoiusly at least have a gut feeling that performance testing is important. And we totally agree with you.

There are a few reasons why we think it's important:

**#1: Performance is a feature!**
It really is! Your users will love you, you will earn more money, and unicorns will dance in joy if you focus on performance. Don't belive us? Well, [this guy says the same](http://www.codinghorror.com/blog/2011/06/performance-is-a-feature.html). Don't you belive him either? He created StackOverflow and stuff, so you should! We won't repeat all the examples here, but suffice to say: this stuff matters! Performance really *is* a feature, a damn important one too!

**#2: Bad performance can kill you!**
Literary! Or figuratively, whatever word kids use these days. The daily press really loves a god "*important site* down, users takes to the streets"-story. You definately don't want your site's logo on those demonstation signs! And when you then have to go into rush-mode and try to fix things, bad things can (and according to mr. Murphy, often does) happen. That's when Kenneth36 and all those nasty things happens! So you definately want to test performance yourself, and don't leave that task to your users on release-day.

**#3: It's good for your health!**
[Some guy](http://en.wikipedia.org/wiki/H._James_Harrington) (with lots of gray hairs, that's how you know he's a smart one) once uttered this brilliant quote: 

> Measurement is the first step that leads to control and eventually to improvement. 
> If you can't measure something, you can't understand it. If you can't understand it, you can't control it. 
> If you can't control it, you can't improve it.

In the end, performance testing really boils down to the basic task of caring about what you do. 
You unit test your code, sweat over small details in your user interface, and even care about *how* you work (Scrum, Kanban and all that stuff). 
Why shouldn't you take the same pride in your product's ability to actually serve your users, and don't fall down when everyone and their grandmas come knocking. 
Well, we think you should!

Enough about that, now that you are properly motivated, we'll put on our "serious-glasses" and start doing some real work:

Why Grinder?
------------

Okay, you get it already, performance testing is important!
But why should you use Grinder in particular?
And what is Grinder anyway?
Lets have a closer look.

Grinder is a load testing framework written in Java.
It is free, open source under a BSD-style licence, and supports large scale testing using distributed load injector machinces.
The main selling point of Grinder, however, is that it is lightweight and easy to use.
With Grinder there are no licences to buy or large environments to set up.
You just write a simple test script and fire up the grinder.jar file.

The reason you, as a developer, will prefer Grinder is that you create your tests in code, not some GUI.
Although Grinder itself is written in Java, the test scripts are written in Jython or Closure, which means you can utilize the expressiveness of a dynamic or functional language while still tapping into the resources of Java and the JVM.
(Of course, should you be so unfortunate not to be a developer, then Grinder might not be the tool for you. But you still need to understand why your developers want and should use Grinder!)

So, lets have a closer look on the nuts and bolts of Grinder.


Grinder 101
-----------

The Grinder framework is comprised of three types of processes (or programs):

1. *Worker processes*: Interprets test scripts and performs the tests using a number of parallel *worker threads*.
1. *Agent processes*: Long running process that starts and stops worker processes as required.
1. *The Console*: Coordinates agent processes, and collates and displays statistics.

![Overview of the Grinder framework](https://raw.github.com/kvalle/grinder-bloggpost/master/images/grinder-overview.png)

In this tutorial we'll keep things simple, and focus on the worker and agent precesses.
We will start an agent process manually, providing it with a test script and configuration, leaving the console and distributed testing out for now.

The test script is simply a Python (or Clojure) source file.
Grinder expects to find a class called `TestRunner` which should implement two methods: `__init__` and `__call__`.
The init-method is called by Grinder initially to set up the test, and call-method is then called repeatedly for each iteration of the testing.