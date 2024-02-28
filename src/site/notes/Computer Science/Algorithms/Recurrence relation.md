---
{"dateCreated":"2022-04-24 23:57","tags":["algorithms","data structures"],"pageDirection":"ltr","dg-publish":true,"permalink":"/computer-science/algorithms/recurrence-relation/","dgPassFrontmatter":true}
---


# Recurrence relation

## what is Recurrence relation
In mathematics, a **recurrence relation** is an [equation](https://en.wikipedia.org/wiki/Equation "Equation") that expresses the
**n** -th term of a [sequence](https://en.wikipedia.org/wiki/Sequence_(mathematics) "Sequence (mathematics)") as a function of the  **k**  preceding terms. 

### examples 
#### merge sort
``` psudo
sort(array A, int begin, int end):
 if(begin = end)
	 return;
 sort(A, begin , (begin + end)/2)
 sort(A, ((begin + end)/2)+1, end)
 pivot = (begin + end)/2 
 Merge(A,begin, pivot, end)	 	 
```
we can represent this algorithm as a recurrence relation in the following way: 

$$ T(n) =
  \begin{cases}
    \theta(1)       & \quad \text{if } n = 1 \\\\
    T\left(\lceil\frac{n}{2}\rceil\right)+ T\left(\lfloor\frac{n}{2}\rfloor\right) + \theta(n)  & \quad \text{if } n>1
  \end{cases}
 $$
 
this formula is the representation of [[Computer Science/Algorithms/Recurrence relation#merge sort\|#merge sort]]. how ever it should be mention that as learned in [[Computer Science/Algorithms/Asymptotic notations\|Asymptotic notations]] we can ignore any constant or round value, so lets rewrite the formula as the following: 

$$T(n)=2T(\frac{n}{2})+\theta(n)\ \ , T(1)=1$$

## How can we calculate a Recurrence relation
###   Recursion tree
lets look at the following relation : 

$$T(n) = 2T\left(\frac{n}{2}\right)+\theta(n)$$

we can calculate $T(n)$ by creating this Recursion tree: 

![Pasted image 20220425015536.png](/img/user/Assets/Pasted%20image%2020220425015536.png)

this will continue until $\frac{n}{n}$ and we are looking for the height of the tree in order to solve $T(n)$

so, lets explain whats going on:
*  each level of the tree is performing $n$ work
* each branch of a level is performing $\frac{n}{2^b}$ where $b$ is the branch level so in total each branch is performing $2^bֿ\cdot\frac{n}{2^b}$
* as we said this tree will expand until $2^b=n$ there for the height of the tree will be $b = log(n)$. 
* now we are performing $\log(n)$ calls where each call perform $n$ work. there fore: $T(n)=\theta(n\cdot\log(n))$ 


#### unequal Recursion tree
lets look at the next relation:

$$T(n)=T(\frac{n}{3})\ + \ T(\frac{2n}{3})\ + \ C_n$$

where $C_n$ is the non recursive part of the function.

this time the tree is not even as the tree we saw before and it will look something like this: 
![recurrsion_tree_ex2.jpeg](/img/user/Assets/recurrsion_tree_ex2.jpeg)

each level is performing $n$ work as before. how ever the right branches clearly is performing more work. what is going to happen is that the more the tree branches will expand, the more job will be done on the rightmost side of the tree and it will become more and more unbalanced. 

as before the tree will end at $\frac{n}{n}$ so that mean the left branch will finish way more quickly. 

let's ask our self : **so what if the tree is not equal? what will happen?** the answer is the following. in the [[Computer Science/Algorithms/Recurrence relation#Recursion tree\|#Recursion tree]] from before, each branch performed $n$ work.
but that only worked because they were equal. in this case at some point the left branch will finish its work and the right one will continue his. 
that means that at some branch $b_i$ what will happen is the following: 
$\forall b\geq b_i$ $work < C_n$   

**so how to calculate $T(n)$?**
* we will divide the tree to 2 parts: the parts where $work=C_n$ and the part where $work<C_n$
the tree will look something like this: 
![Pasted image 20220425014104.png](/img/user/Assets/Pasted%20image%2020220425014104.png)

* now we going to calculate the maximum height of the tree in the first part and the maximum height in the second part. 
* the first part will be the best case scenario of our function and therefore will be $\Omega$  
* the second part will be the worst case scenario of our function and therefore will be $O$ .

* the __equal__ tree height will be $log_{3}n$ and the __unequal__ tree height will be $log_{2/3}n$ 
* so as we said the first part [[time complexity\|time complexity]] will be $\Omega(n\log_{3}n)$ and the second part will be $O(n\log_{2/3}n)$ but we know from [[Computer Science/Algorithms/Asymptotic Analysis\|Asymptotic Analysis]] that switching between bases is performed by constant and there for the total time complexity of this function will be $\theta(n\cdot\log n)$    


#### Recursion tree with more than 2 branches
lets check the following example: 

$$T(n)=3T(\frac{n}{4})+Cn^2$$

where $Cn^2$ is some work inside the function that is non recursive.

the tree will look something like this: 
![Pasted image 20220425084636.png](/img/user/Assets/Pasted%20image%2020220425084636.png)
lets see whats going on : 
* because the work inside the recursion is divisive each branch will do less work until $T(1)$  
* so we need to understand 2 things : the first one is the height of the tree so let's build a formula assuming $b_i$ is the branch $i$ in the tree. and we know that  $\text{at branch: } b_n\ \text{the work is: }\frac{n}{n}=1$  
   the formula will be as the following : 
   at each branch $b_i$ each leaf will do the following job $c(\frac{n}{4^{i}})^2$ the first $i$ is 0.
    
* each branch will do the sum of that that will calculate to: $(\frac{3}{16})^{i}\cdot Cn^2$ 
* if we sum all the floors we will get : 
 
$$Cn^2\cdot\sum_{n=0} ^{\log_{4}n} \left(\frac{3}{16}\right)^{i}=O(n^2)$$

* this is a [Geometric progression](https://en.wikipedia.org/wiki/Geometric_progression) where $q<1$ and therefore it will sum up to a constant.

 * the best case scenario is $\Omega(n^2)$ because that the job the first branch is doing
 * because the sum is the worst case scenario and the first work in the best and they are both of $n^2$  the [[Computer Science/Algorithms/Asymptotic notations\|Asymptotic notations]] of this formula will be :  $\theta(n^2)$ 


### Iterative method
in this method we will __"open"__ the formula to get an idea if what will happen at the end. (we need to now the value of $T(1)$ for that but it most of the time will be 1 or 0).

#### example 1

$$T(n) = 2T(n-3)+2$$
lets open the formula:

$$T(n-3)=2T(n-6)+2$$
$$T(n-6)=2T(n-9)+2$$

lets see how its sums up in $T(n)$ :
$$T(n)=4(2T(n-9)+2)+4+2=8T(n-9)+8+4+2$$
if we try to build it until the end we will get :

$$T(n)=2^{i}T(n-3\cdot i)+2^{i+1}-2$$ 
notice that $2^{i+1}-2$ is a [Geometric progression](https://en.wikipedia.org/wiki/Geometric_progression) 

**how ever, this is not a valid proof and we need to proof it with [# Inductive reasoning](https://en.wikipedia.org/wiki/Inductive_reasoning)** 

* ==base-==  $i=1$  we will get   $T(n)=2T(n-3)+2$ as needed.
* ==step-==   assume that $i=k-1$ and we will prove for $i=k$ 
**we will use full induction most of the times**
$$T(n)=2^{k-1}ֿ\ T(n-(k-1)\cdot3)+2^k+2$$
continue develop the expression and we will get the above result.
because we know that $T(0)=T(1)=T(2)=T(3)=1$
now assume $i=\frac{n-1}{3}$ and we now get the following : $T(n)=2^{\frac{n}{3}}\cdot T(1)+2^{\frac{n}{3}+1}-2=2^{\frac{n}{3}}+2^{\frac{n}{3}+1}-2\in \theta(2^{\frac{n}{3}})$  

### positioning method
 we will guess what is the big $O$ notation and the $\Omega$ notation, by guessing i mean we will assume that there is a constant $C$ according to [[Computer Science/Algorithms/Asymptotic Analysis\|Asymptotic Analysis]] rules. 
* lets assume that $T(n)\in O(n^2)$ which means that $\exists \ C,n_0 \ \text{such that: }\forall n\geq n_0\text{,}\ T(n)\leq C\cdot n^2$         
* proof you assumption with **inductive reasoning** 
* do the same for $\Omega$ if possible.
* try the same for a smaller big $O$ and $\Omega$ 


###  the Master theorem
the master theorem help us solve a specific type of Recurrence relation but is the fastest way from all the above. 

for formulas that look like the following:

$$T(n)=aT(\frac{n}{b})+f(n)\text{ where: }a\geq1,\ b>1$$

where $f(n)$ is the root of the function (the non recursive part). 
the base for the master theorem is simple **find out who from the root or the recursion part does more work.** 
we can achieve this with the following rules: 
#### case 1:

$$f(n)\in O(n^{log_{b}a-\epsilon})\to T(n)=\Theta(n^{log_{b}a})$$

this is the case where the recursive part works harder than the root

#### case 2:

$$f(n)\in \Theta(n^{log_{b}a})\to T(n)=\Theta(n^{log_{b}a}\ log\ n)$$

this is the case when both the recursive part and the root does the same work 

there is an extended case for that one :
![Pasted image 20221130214437.png](/img/user/Assets/Pasted%20image%2020221130214437.png)

#### case 3: 

$$f(n)\in \Omega(n^{log_{b}a+\epsilon})\to T(n)=\Theta(f(n))$$

this case is when the root does more work than the recursive part.

#### examples:
1) 
 $T(n)=8T(\frac{n}{4})+n^2$ 
 $f(n)=n^2$ , $a=8,\ b=4$ 
 $\log_{b}a = \log_{4}8 =1.5$ 

 $n^{1.5}<n^{2}\ \ \to \forall_{0<\epsilon <0.5}: \ \ \ f(n)=n^2=\Omega(n^{1.5+\epsilon})$    

therefore:
$T(n)\in\Theta(f(n))\in\Theta(n^2)$ 

2) 
$T(n)=9T(\frac{n}{3})+n^2$
$log_{3}9=2,\ \ \ f(n)=n^{2}=\Theta(n^{\log_{3}9})$ 

therefore: $T(n)\in\Theta(n^{2}logn)$ 

### switching variables method
lets look at the following relation: 

$$T(n)=2T(\sqrt{n})+logn$$

this can be a hard expression to develop using the methods above..
we should try make it easier by switching variables:

mark $m=logn$ and notice that: $2^m=n$ now:

$$\displaylines{T(2^m)=2T(2^{\frac{m}{2}})+m
\\  \text{mark: }S(m)=T(2^m)
\\ \downarrow \\
S(m)=2S(\frac{m}{2})+m}$$
this is a recurrence relation we already know $S(m)\in \Theta(m\cdot logm)$ 
therefore: 

$$T(n)=T(2^{m})=S(m)=\Theta(m\log m)=\Theta(\log(n)\log(\log(n)))$$


