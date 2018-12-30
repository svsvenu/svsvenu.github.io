---
layout: post
title: The mysterious memory mapped file
---


## Assumptions

This blog assumes that you have had to write something to a file in java, ideally you have had to write a lot to 
a file using some of the native API's.

- Docker daemon is running ( In my case i am running docker for mac )
- Openshift is running ( I am using minishift but you could potentially use the real version )
- oc tool ( The client for accessing openshift ) is in the path and can be accessed from any terminal window


## Introduction

At a high level a write fo file process for me involves a visit to google,a copy of 
code from stack overflow, mess up closing some of the streams, fix the code by calling the close
method on every class that offers it and get on with life. Recently, i have had to deal with writing and 
reading a lot of data which got me looking into options for performance improvement and went down this
rabbit hole of how data is read from the hard drive, the role the operating system plays and some of the
API's that give incredible performance. 


## The traditional process

The innocuous write method call is hiding a lot of chaos that ensues. The jvm makes a system call to the kernel,
 requiring a copy of the data from the user space to the kernel space. When you think about it, 
 the sole reason for the copy is for the kernel to get to it as it has its own address space. 
 The kernel then, simplistically said, dumps the data to the drive. Any one now can see the benefit of avoiding
 this copy wondering , if only the kernel could see the file contents. While the average like me wondered,   
 the gifted accomplished it. Memory mapping ( now staring to make more sense isn’t it ) is precisely that, 
 a common location where the kernel and user can see the same location. I believe, that a “page fault” like 
 mechanism keeps this shared memory and device in sync.


## Application

This Java application codes to run in two modes, memory mapped mode ( avoiding the system call ) and “regular mode” 
where blissfully unaware programmers got the code out of stack over flow. Now, you must have gotten this far in 
the blog thinking how is he going to prove/disprove that a system call is made? Not made ? 
There must be a way ? Yep, enter “ strace”, tells you the system calls made by the process, 
at this point our tool box is full of tools and body reeking of caffeine.
Starting the application in memory mapped mode and tracking the system calls using strace with the following 
commands shows us that there aren’t any system calls being made while the data is still being written as 
shown below. Now starting the application in regular mode shows that it’s making shit tons of system calls.

## Conclusion

While the effects of this behavior might not be noticeable at smaller loads, the savings do increase exponentially 
as the load spikes, volume wise. System calls are the middleware you need to have most of the time, 
this scenario is one of the outliers where bypassing it might be beneficial.

## The code

You can find the code here [Da project](https://github.com/svsvenu/poc/tree/master/camel-standalone-rest)



