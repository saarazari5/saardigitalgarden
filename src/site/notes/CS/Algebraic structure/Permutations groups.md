---
{"dateCreated":"2023-02-02 00:23","tags":["abstract_algebra","computer_science"],"pageDirection":"rtl","dg-publish":true,"permalink":"/cs/algebraic-structure/permutations-groups/","dgPassFrontmatter":true}
---


# חבורת התמורות
לכל $n\in\mathbb{N}$ נגדיר את __חבורת התמורות__ $S_{n}$ להיות אוסף כל הפונקציות ההפיכות מ $[n]\to[n]$  כאשר הפעולה של $S_{n}$ היא $\circ$ כלומר הרכבה.
באופן כללי נוכל לסמן $S_{X}$ להיות קבוצת הפונקציות ההפיכות ב $X^{X}$ .

למשל [[CS/Algebraic structure/Permutations\|התמורה]] 

$$f=\begin{pmatrix}1 & 2 & 3 & 4 \\ 4 & 2 & 1 & 3\end{pmatrix}$$

היא תמורה השייכת לחבורה $S_{4}$ .


__משפט__:
תהי $S_{n}$ ותהיינה $f,g\in S_{n}$ אזי 

$$sign(f\circ g)= sign(f)\cdot sign(g)$$

_הוכחה:_

$$\displaylines{sign(f\circ g)= \prod_{i\neq j} \frac{f(g(i))-f(g(j))}{i-j}\\= \prod_{i\neq j}\frac{f(g(i))-f(g(j))}{i-j}\cdot \frac{g(i)-g(j)}{g(i)-g(j)}\\
= \prod_{i\neq j}\underbrace{\frac{f(g(i))-f(g(j))}{g(i)-g(j)}}_{sign(f)}\cdot \underbrace{\frac{g(i)-g(j)}{i-j}}_{sign(g)}=sign(f)\cdot sign(g)
} $$

==משפט==
__הסדר של $S_{n}$__  הוא $n!$ שכן מ [[CS/discrete math/combinatorics basics\|קומבינטוריה]] נוכל לבחור לכל ערך ב $[n]$ בהתחלה $n$ איברים ולאחר מכן $n-1$ וכן הלאה..

## מחזור
יהי $n\in\mathbb{N}$ ויהיו $a_{1},\dots,a_{k}\in [n]$ כך ש $\forall_{i\neq j\in[k]}:a_{i}\neq a_{j}$ . נגדיר את המחזור $f=(a_{1} \ a_{2} \ a_{3}\dots a_{k})\in S_{n}$ להיות:

$$\forall_{i\in[k-1]}:f(x)=\begin{cases}
x & x\neq a_{i} \\ \\
a_{i+1}&x=a_{i} \\ \\
a_{1}&x=a_{k}
\end{cases}$$

>[!note] הערה
>למחזור באורך $2$ קוראים חילוף, האורך של מחזור $f=(a_{1} \ a_{2} \ a_{3}\dots a_{k})\in S_{n}$ הוא $k$ 


__מחזורים זרים__ הם מחזורים שאין להם מספר משותף. 
==__הבחנה:__ מחזורים זרים הם מתחלפים זה עם זה== (לא בעיה להראות למה) 

==נשים לב: כל תמורה אפשר להציג כהרכבה של מחזורים זרים וכל מחזור אפשר להציג כהרכבה של חילופים== .

למשל 

$$\begin{pmatrix}1 & 2 & 3 & 4\\2 & 1 & 4 & 3  \end{pmatrix} = (1 \ \ 2)(3 \ \ 4)$$

__משפט__
$$\begin{pmatrix}a_{1} & a_{2} & \dots & a_{k}\end{pmatrix}=(a_{1} \ \ a_{2})(a_{2} \ \ a_{3})\cdots (a_{k-1} \ \ a_{k})$$

_הוכחה-_
נוכיח שיש שיוויון בין שתי פונקציות כלומר שהן שולחו כל איבר לאותו המקום. נחלק למקרים 

$x\neq a_{i}$ : 

$$\begin{pmatrix}a_{1} & a_{2} & \dots & a_{k}\end{pmatrix}(x)= (a_{1} \ \ a_{2})(a_{2} \ \ a_{3})\cdots (a_{k-1} \ \ a_{k})(x)=x$$

$x=a_{i}$ :

$$\begin{pmatrix}a_{1} & a_{2} & \dots & a_{k}\end{pmatrix}(a_{i})=a_{i+1}$$

$$\displaylines{
(a_{1} \ \ a_{2})(a_{2} \ \ a_{3})\cdots (a_{k-1} \ \ a_{k})(a_{i})=(a_{1} \ \ a_{2})(a_{2} \ \ a_{3})\cdots (a_{i} \ \ a_{i+1 })(a_{i})\\
= (a_{1} \ \ a_{2})(a_{2} \ \ a_{3})\cdots (a_{i-1} \ \ a_{i})(a_{i+1})= a_{i+1}
}$$

## סימן החילוף
עבור $f$ חילוף נוכיח שהסימן הוא $-1$ ולכן הסימן של מחזור הוא 

$$sign(a_{1}\dots a_{k})= sign((a_{1}\ a_{2})\cdots(a_{k-1}\ a_{k}))= (-1)^{k-1}$$

ההוכחה לנ״ל תהיה על סימן החילוף הספציפי $(1,2)$ ועל כל אחד אחר ההוכחה תהיה זהה:

$$sign(f)= \frac{2-1}{1-2}\cdot \frac{2-3}{1-3}\cdots \frac{2-n}{1-n}\cdot \frac{1-3}{2-3}\cdots \frac{1-2}{2-n}\cdots = \frac{2-1}{1-2}=-1$$

המעבר האחרון נובע מכך שאחרי שחישבנו את את המכפלות עבור הערכים $1,2$ נקבל פשוט כפל של 1 כי האיברים יצטמצמו לנו.

>[!note] הבחנה
>אנחנו יודעים שכל [[CS/Algebraic structure/Cyclic groups\|החבורות הציקליות]] הן גם אבליות ולכן מcontra positive של הטענה יתקיים שאם חבורה אינה אבלית היא גם אינה ציקלית, קל לראות ש $S_{n}$ אינה ציקלית לכל $n\geq 3$ כי אלו לא אבליות.


## חבורת החילופין
חבורת החילופין (נקראת גם חלופת התמורות הזוגיות) $A_{n}$ היא תת החבורה הבאה של $S_{n}$ 

$$A_{n}=\{\sigma\in S_{n} \ | \ sign(\sigma) = 1\}$$

הסדר של החבורה הזו הוא $\frac{n!}{2}$. נשים לב ש $A_{n}=ker(sign)$ כי אנחנו יודעים שהסימן הוא הומומורפיזם.