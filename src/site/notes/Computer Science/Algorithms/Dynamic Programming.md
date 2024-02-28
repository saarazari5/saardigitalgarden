---
{"dateCreated":"2022-06-14 10:08","tags":["algorithms","data_structures"],"pageDirection":"ltr","dg-publish":true,"permalink":"/computer-science/algorithms/dynamic-programming/","dgPassFrontmatter":true}
---


# Dynamic Programming
[[Computer Science/Algorithms/Dynamic Programming Problems\|Dynamic Programming]] is mainly an optimization over plain [recursion](https://www.geeksforgeeks.org/recursion/). Wherever we see a recursive solution that has repeated calls for same inputs, we can optimize it using Dynamic Programming. The idea is to simply store the results of subproblems, so that we do not have to re-compute them when needed later. This simple optimization reduces time complexities from exponential to polynomial. For example, if we write simple recursive solution for [Fibonacci Numbers](https://www.geeksforgeeks.org/program-for-nth-fibonacci-number/), we get exponential time complexity and if we optimize it by storing solutions of subproblems, time complexity reduces to linear.


![Pasted image 20220614100839.png](/img/user/Assets/Pasted%20image%2020220614100839.png)

## How to solve a Dynamic Programming Problem ?

```
Steps to solve a DP

1) Identify if it is a DP problem
2) Decide a state expression with 
   least parameters (can be a data structure of some sort)
3) Formulate state relationship    
4) Do tabulation (or add memoization)
```

## how to identify a DP ? * write a naive solution. 
* write a recursive solution.
* check if there are multiple values that are being checked or calculated.

## Deciding the state   
DP problems are all about state and their transition. This is the most basic step which must be done very carefully because the state transition depends on the choice of state definition you make. So, let’s see what do we mean by the term “state”.

**State** A state can be defined as the set of parameters that can uniquely identify a certain position or standing in the given problem. This set of parameters should be as small as possible to reduce state space. 

So, our first step will be deciding a state for the problem after identifying that the problem is a DP problem.  
As we know DP is all about using calculated results to formulate the final result.   
So, our next step will be to find a relation between previous states to reach the current state.

## Where should dynamic programming be used?
Dynamic programming is used when one can break a problem into more minor issues that they can break down even further, into even more minor problems. Additionally, these subproblems have overlapped. That is, they require previously calculated values to be recomputed. With dynamic programming, the computed values are stored, thus reducing the need for repeated calculations and saving time and providing faster solutions. 

## How Does Dynamic Programming Work?
Dynamic programming works by breaking down complex problems into simpler subproblems. Then, finding optimal solutions to these subproblems. Memorization is a method that saves the outcomes of these processes so that the corresponding answers do not need to be computed when they are later needed. Saving solutions save time on the computation of subproblems that have already been encountered. 

Dynamic programming can be achieved using two approaches:

###  Top-down approach
In computer science, problems are resolved by recursively formulating solutions, employing the answers to the problems’ subproblems. If the answers to the subproblems overlap, they may be memoized or kept in a table for later use. The top-down approach follows the strategy of memorization. The memoization process is equivalent to adding the recursion and caching steps. The difference between recursion and caching is that recursion requires calling the function directly, whereas caching requires preserving the intermediate results.

The top-down strategy has many benefits, including the following:

* The top-down approach is easy to understand and implement. In this approach, problems are broken down into smaller parts, which help users identify what needs to be done. With each step, more significant, more complex problems become smaller, less complicated, and, therefore, easier to solve. Some parts may even be reusable for the same problem.

* It allows for subproblems to be solved upon request. The top-down approach will enable problems to be broken down into smaller parts and their solutions stored for reuse. Users can then query solutions for each part. 

* It is also easier to debug. Segmenting problems into small parts allows users to follow the solution quickly and determine where an error might have occurred. 
* 
Disadvantages of the top-down approach include:
* The top-down approach uses the recursion technique, which occupies more memory in the call stack. This leads to reduced overall performance. Additionally, when the recursion is too deep, a stack overflow occurs. 

### Bottom-up approach
In the bottom-up method, once a solution to a problem is written in terms of its subproblems in a way that loops back on itself, users can rewrite the problem by solving the smaller subproblems first and then using their solutions to solve the larger subproblems. 

Unlike the top-down approach, the bottom-up approach removes the recursion. Thus, there is neither stack overflow nor overhead from the recursive functions. It also allows for saving memory space. Removing recursion decreases the time complexity of recursion due to recalculating the same values. 

The advantages of the bottom-up approach include the following:

* It makes decisions about small reusable subproblems and then decides how they will be put together to create a large problem. 

* It removes recursion, thus promoting the efficient use of memory space. Additionally, this also leads to a reduction in timing complexity. 

## Signs of dynamic programming suitability
Dynamic programming solves complex problems by breaking them up into smaller ones using recursion and storing the answers so they don’t have to be worked out again. It isn’t practical when there aren’t any problems that overlap because it doesn’t make sense to store solutions to the issues that won’t be needed again.

Two main signs are that one can solve a problem with dynamic programming: subproblems that overlap and the best possible substructure.

__Overlapping subproblems__

When the answers to the same subproblem are needed more than once to solve the main problem, we say that the subproblems overlap. In overlapping issues, solutions are put into a table so developers can use them repeatedly instead of recalculating them. The recursive program for the Fibonacci numbers has several subproblems that overlap, but a binary search doesn’t have any subproblems that overlap.

A binary search is solved using the divide and conquer technique. Every time, the subproblems have a unique array to find the value. Thus, binary search lacks the overlapping property. 

For example, when finding the nth Fibonacci number, the problem F(n) is broken down into finding F(n-1) and F. (n-2). You can break down F(n-1) even further into a subproblem that has to do with F. (n-2).In this scenario, F(n-2) is reused, and thus, the Fibonacci sequence can be said to exhibit overlapping properties. 

__Optimal substructure__

[[Computer Science/Algorithms/greedy algorithms\|The optimal substructure property]] of a problem says that you can find the best answer to the problem by taking the best solutions to its subproblems and putting them together. Most of the time, recursion explains how these optimal substructures work.

This property is not exclusive to dynamic programming alone, as several problems consist of optimal substructures. However, most of them lack overlapping issues. So, they can’t be called problems with dynamic programming.

You can use it to find the shortest route between two points. For example, if a node p is on the shortest path from a source node t to a destination node w, then the shortest path from t to w is the sum of the shortest paths from t to p and from p to w.

Examples of problems with optimal substructures include the longest increasing subsequence, longest palindromic substring, and longest common subsequence problem. Examples of problems without optimal substructures include the most extended path problem and the addition-chain exponentiation. 