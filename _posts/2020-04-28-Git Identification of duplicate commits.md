---
layout: post
title: Git identification and elimination of duplicate commits
---


## Assumptions

This blog assumes that you know the basics of GIT as soucre control and are familiar with the concepts
of merge and rebase.


## Introduction

I bet google has a dedicated service center to cater to rebase vs merge blogs. It seems to 
spark an un-usual desire to share. So here i am, another face in the crowd. 


## Death taxes and new commits

Speaking of inevitable certainties, the book will train you that a rebase will take the 
branch you are rebasing on, go to its tip, 'replay' all your work on top of it. Its the 
time distorted equivalent of you started to code now, and finished your work on top of the latest
that the other guys have done. Every commit that you have done from the merge-base to now will ( should )
show up in the commit log after rebase

So highlevel steps might be

1. start with a common base, this can be determined by the following command 
		* git merge-base dev master
		
2. Advance to the tip of the branch rebasing on

3. Apply all the commits on the current branch starting at the merge base


## Application

A master branch with one commit ( MAS1 )
A dev branch that is cut at the first master commit (MAS1) and has 3 commits of its own (DEV1, DEV2, DEV3)
A feature branch that is cut at the first feature commit ( dev1) and has 3 commits of its own ( FEAT1, FEAT2, FEAT3 ).
An embedded feature branch that is cut at the first feature commit ( FEAT1 ) and has 3 commits of its own ( EMBFEAT1 , EMBFEAT2 )

Each commit will have a file and the contents same as the file name, for example the commit MAS1 will have a file called MAS1
with content of MAS1 and a comment message that says "Added MAS1 to master. 

The following script will set up the above git branch structure


![_config.yml]({{ site.baseurl }}/images/rebase.png)
 

## Conclusion

While the effects of this behavior might not be noticeable at smaller loads, the savings do increase exponentially 
as the load spikes, volume wise. System calls are the middleware you need to have most of the time, 
this scenario is one of the outliers where bypassing it might be beneficial.

## The code

You can find the code here [Da project](https://github.com/svsvenu/poc/tree/master/memory-mapped-files)



