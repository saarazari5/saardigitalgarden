---
{"dateCreated":"2022-10-29 18:12","tags":["algorithms","data_structures"],"pageDirection":"ltr","dg-publish":true,"permalink":"/computer-science/algorithms/asymptotic-analysis/","dgPassFrontmatter":true}
---



# Asymptotic Analysis

> [Asymptotic Analysis](http://en.wikipedia.org/wiki/Asymptotic_analysis) is defined as the big idea that handles the above issues in analyzing algorithms. In Asymptotic Analysis, we **evaluate the performance of an algorithm in terms of input size** (we don’t measure the actual running time). We calculate, how the time (or space) taken by an algorithm increases with the input size.

## Why performance analysis? 
There are many important things that should be taken care of, like user-friendliness, modularity, security, maintainability, etc. Why worry about performance?  The answer to this is simple, we can have all the above things only if we have performance. So performance is like currency through which we can buy all the above things. Another reason for studying performance is – speed is fun! To summarize, performance == scale. Imagine a text editor that can load 1000 pages, but can spell check 1 page per minute OR an image editor that takes 1 hour to rotate your image 90 degrees left OR … you get it. If a software feature can not cope with the scale of tasks users need to perform – it is as good as dead.

## Given two algorithms for a task, how do we find out which one is better? 

One naive way of doing this is – to implement both the algorithms and run the two programs on your computer for different inputs and see which one takes less time. There are many problems with this approach for the analysis of algorithms. 

-   It might be possible that for some inputs, the first algorithm performs better than the second. And for some inputs second performs better. 
-   It might also be possible that for some inputs, the first algorithm performs better on one machine, and the second works better on another machine for some other inputs.

> [Asymptotic Analysis](http://en.wikipedia.org/wiki/Asymptotic_analysis) is the big idea that handles the above issues in analyzing algorithms. In Asymptotic Analysis, we **evaluate the performance of an algorithm in terms of input size** (we don’t measure the actual running time). We calculate, how the time (or space) taken by an algorithm increases with the input size.

