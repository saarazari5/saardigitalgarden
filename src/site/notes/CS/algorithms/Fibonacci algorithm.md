---
{"dateCreated":"2022-05-04 17:51","tags":["algorithms","data_structures"],"dg-publish":true,"pageDirection":"ltr","permalink":"/cs/algorithms/fibonacci-algorithm/","dgPassFrontmatter":true}
---


# Fibonacci algorithm 

we know the ordinary way of calculation with the formula : 
$f_{n}=f_{n-1}+f_{n-2}$ 
however the runtime of this algorithm is $O(2^n)$ 

if we calculating from the first elements to top it will be $\Theta(n)$ but with space complexity of $O(n)$ because we need to create $n$ variables for calculation ,but we can do even better.

$$f_n=\frac{(\frac{1+\sqrt{5}}{2})^{n}-(\frac{1-\sqrt{5}}{2})^{n}}{\sqrt{5}}$$

this formula is one way of calculation the $n$-th element in fib series, the time complexity of raising a number to a power of $n$ is $O(logn)$ how ever when working with a irrational numbers there can be a miscalculations, even more when $n$ is a very large number.

to fix this we can use the following method-

we can improve the miscalculation by looking at $f_{n+1}$ and $f_{n}$ 
as a vector and we will assume the following math equation (we can prove it with induction): 
$$ \bigl(\begin{smallmatrix}
f_{n+1} \\ f_{n} 
\end{smallmatrix} \bigr) = \bigl(\begin{smallmatrix}
1&1 \\ 1&0
\end{smallmatrix} \bigr)\bigl(\begin{smallmatrix}
f_{n} \\ f_{n-1}
\end{smallmatrix} \bigr) $$

we can open this formula and we will get 
$$ \bigl(\begin{smallmatrix}
f_{n+1} \\ f_{n} 
\end{smallmatrix} \bigr) = \bigl(\begin{smallmatrix}
1&1 \\ 1&0
\end{smallmatrix} \bigr)^i\cdot\bigl(\begin{smallmatrix}
f_{n+1-i} \\ f_{n-i}
\end{smallmatrix} \bigr) = \bigl(\begin{smallmatrix}
1&1 \\ 1&0
\end{smallmatrix} \bigr)^n\cdot\bigl(\begin{smallmatrix}
1 \\ 0
\end{smallmatrix} \bigr) $$
because matrix multiplication is $\Theta(1)$ the entire calculation is just $\Theta(logn)$ because raising to the power of numbers mentioned as above is $\Theta(logn)$.


