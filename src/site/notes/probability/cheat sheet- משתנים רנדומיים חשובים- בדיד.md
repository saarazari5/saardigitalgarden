---
dateCreated: "2022-12-10 17:19"
tags: [probability, computer_science]
dg-publish: true
dg-home: false
---

# cheat sheet- משתנים רנדומיים חשובים- בדיד
להסבר מפורט יותר: [[משתנים רנדומיים-בדיד]] 
## אחיד
ב $[a,b]$ :
$$p_{X}(k)= \begin{cases} \frac{1}{b-a+1}& k\in \{a,a+1,\dots,b\} \\\\ 0&else \end{cases}$$
$$E[X]= \frac{a+b}{2}$$
$$var(X)= \frac{(b-a)(b-a+2)}{12}$$


## ברנולי
עבור פרמטר $p$ המייצג הסתברות להתרחשות של מאורע או הצלחה של ניסוי.
$$p_{X}(k)= \begin{cases} p&k=1 \\ 1-p&k=0 \end{cases}$$
$$E[X]=p$$
$$var(X)=p(1-p)$$

## בינומי
עבור $p$ ו $n$ 
$$p_{X}(k)= \binom{n}{k}p^{k}(1-p)^{n-k} \ \ \ \ k=0,1,\dots,n$$
$$E[X]=np$$
$$var(X)= np(1-p)$$




## גיאומטרי 
עבור פרמטר $p$ ההסתברות להצלחה בניסוי 
$$p_{X}(k)= (1-p)^{k-1}p \ \ \ \ \ k\in \mathbb{N}$$
$$E[X]= \frac{1}{p}$$
$$var(X)= \frac{1-p}{p^{2}}$$



##  פואסון
עם פרמטר $\lambda$ (כאשר $n$ גדול ממש ו$p$ קטן ממש נקבל $\lambda = np$) 
$$\forall_{k\in \mathbb{N}} : p_{X}(k) = e^{-\lambda}\cdot \frac{\lambda^{k}}{k!}$$
$$E[X]=var(X)=\lambda$$



