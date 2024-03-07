---
{"dateCreated":"2022-05-06 01:17","tags":["data_structures"],"pageDirection":"ltr","dg-publish":true,"permalink":"/computer-science/data-structures/linked-list/","dgPassFrontmatter":true}
---


# Linked list
A linked list is a [[Computer Science/Data Structures/Linear Data Structures\|Linear Data Structures]], in which the elements are not stored at contiguous memory locations. The elements in a linked list are linked using pointers as shown in the below image:  
![Linkedlist Data Structure](https://media.geeksforgeeks.org/wp-content/cdn-uploads/gq/2013/03/Linkedlist.png "Click to enlarge")

In simple words, a linked list consists of nodes where each node contains a data field and a reference(link) to the next node in the list.

#### indexing in linkedList
indexing in linked list is $O(n)$ .

#### replacing elements
unlike in a regular array when appending new elements require to copy the data sometimes which can be expensive. the linear list allow us to add a new element at index with no further consequences.


### merging linked lists
![Pasted image 20220506012314.png](/img/user/Assets/Pasted%20image%2020220506012314.png)
we have 2 linked list as above with a merging point $x$ we want to find it. lets examine some of the possible methods to do so : 

* calculate : $k+n$ and $l+n$ the length of $L_1$ and $L_2$ 
* the difference will will be $k-l$ if its $<0$ than $k<l$ otherwise $>0$ $k>l$ 
* pick the $max(l,k)$ and iterate the list $k-l \ \ or \ \ l-k$ times. 
* now their $L_1$and $L_2$ have equal length with the $min(k,l)$ and now iterating through each list this min value will give us $x$ as needed. 

time complexity : 

the first step will be $k+n+l+n$ steps just to calculate the size of each list. 
and than iterating again $|k-l|$ times and at the end another $min(k,l)$ times to reach $x$.
therefore the time complexity will be $O(k+l+n)$.

__is there a better way to reach the same result?__
we can perform the same algorithm in $O(d^2)$ and $O(d)$ when $d=max(l,k)$.

#### $O(d^2)$:
find $max(l,k)$ (for this example we will assume $d=k$).
iterate through $L_1$ when each iterator $i$
we will move $L_2$  , $i$ times and check all the elements with $index\leq{i}$ and we compare the elements with the $i$ element in $L_1$.

time complexity : 
$d+\sum_{i=0}^{d}{i}=d+\frac{d\cdot{(0+d)}}{2}=\frac{2d+d^2}{2}<2d+d^2<3d^2\in{\Theta(d^2)}$
#### $O(d)$:
we will perform the same method as above but we will only work with elements where indexes are in jumps of power of  $2^i$ .

in this method what will happen is [Geometric progression](https://en.wikipedia.org/wiki/Geometric_progression) instead of [Arithmetic progression](https://en.wikipedia.org/wiki/Arithmetic_progression)
now $x$ index marked as $k$ will be $\leq{2^i}$ 

now we "create" a new $L_1$ and $L_2$ with only the elements at indexes that are at $2^i$ indexes. 
the problem got smaller now! cool right? 


## identify a circular linked list
![Pasted image 20220506111109.png](/img/user/Assets/Pasted%20image%2020220506111109.png)
* if we can mark every element that passed we can do this in $O(n)$.
 how ever the space complexity will also be $O(n)$.

* if we want to solve this without the extra space, we can check every element index and see if there are more than one index for the same element (by reference no value).

the element which start a circular linked list will always have 2 indexes, the first is $n+1$ and the second is $i<n+1$ where $n$ is the size of the list.


each element we will assume that his index $i=n+1$ . and we iterate so we iterate while assuming the length of the list is i and check if we have 2 indexes for same element. 

the time complexity is :
$\sum\limits_{i=0}^{n}{i}\in{O{(n^2)}}$ 

and if we perform the $2^i$ jumps on the same algorithm we can shrink the problem to be $O(n)$ 

another way is to iterate the list in a rabbit and turtle style :
one iterator will run 1 element at a time and the rabbit will jump 2 elements at a time ... they will surly meet in the circle. this will be a time complexity of $O(n)$. 

now if we want to find the first point which the circle start. 
the algorithm will be : 
* take the point of intersection between the rabbit and the turtle.
* put a turtle iterator at the very start of the list and in the point above. 
* the point where they meet will be the needed point.

