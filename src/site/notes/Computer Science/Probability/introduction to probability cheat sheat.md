---
{"dateCreated":"2023-12-10 17:19","tags":["probability","computer_science"],"pageDirection":"rtl","dg-publish":true,"permalink":"/computer-science/probability/introduction-to-probability-cheat-sheat/","dgPassFrontmatter":true}
---




# מודלים הסתברותיים

א) מרחב מדגם $\Omega$ קבוצת כל התוצאות האפשריות של הניסוי.

ב) חוק הסתברות שמצמיד בין מאורע $A$ להסתברות שלו $P(A)$ שהיא אי שלילית ומייצגת את הסיכוי להתרחשות האיברים בתוך $A$ .

ג) __תוצאות הן תמיד זרות אחת לשנייה__ 

## אקסיומות ההסתברות
א) $P(A)\geq 0$ 

ב) אדטיביות- בהינתן ש$A,B$ הן קבוצות זרות : $P(A\cup B)=P(A)+ P(B)$  (עובד גם על $n$ מאורעות זרים).

ג) נורמליזציה $P(\Omega)=1$ 

ד) חוק ההסתברות הבדיד- אם מרחב המדגם מכיל מספר סופי או בן מנייה של תוצאות אז ההסתברות של כל מאורע $\{s_{1}\dots s_{n}\}$ יהיה

$$P(\{s_{1},\dots,s_{n}\})=P(s_{1})+\dots+ P(s_{n})$$
ה) אם כל $n$ התוצאות הן בסבירות שווה לקרות אזי 

$$P(A)= \frac{|A|}{n}$$

## תכונות נוספות
$$\displaylines{
A\subset B\rightarrow P(A)\leq P(B) \\\\
P(A\cup B)= P(A)+P(B)-P(A\cap B) \\\\
P(A\cup B)\leq P(A)+ P(B)\\\\
P(A\cup B\cup B)= P(A)+ P(A^{c}\cap B)+ P(A^{c}\cap B^{c}\cap C)
}$$



# התסברות מותנת

א)
$$P(A|B)= \frac{P(A\cap B)}{P(B)}$$
התנייה זה הסתברות לכל דבר תחת עולם חדש שהוא במקרה הנ״ל $B$ .

ב) במרחב מדגם שווה הסתברות 

$$P(A|B)= \frac{|A\cap B|}{|B|}$$
ג)
$$P(A|B)= 1- P(A^{c}|B)$$

## חוק הכפל

$$P(\cap_{i=1}^{n} A_{i})= P(A_{1})\cdot \frac{P(A_{1}\cap A_{2})}{P(A_{1})}\cdots \frac{P(\cap_{i=1}^{n}A_{i})}{P(\cap_{i=1}^{n-1}A_{i})} $$

![Pasted image 20221106144043.png](/img/user/Assets/Pasted%20image%2020221106144043.png)

התמונה מקלה לראות את החוק עבור 2 מאורעות למשל , 
$$P(A_{1}\cap A_{2})= P(A_{1})\cdot P(A_{2}|A_{1})$$
# משפט ההסתברות השלמה וחוק בייס

## משפט ההסתברות השלמה 

$$P(B)= \sum\limits_{i=1}^{n}P(A_{i}\cap B)= \sum\limits_{i=1}^{n}P(A_{i})P(B|A_{i})$$

## חוק בייס

$$P(A_{i}|B)= \frac{P(A_{i})P(B|A_{i})}{P(B)}=\frac{P(A_{i})P(B|A_{i})}{\sum\limits_{i=1}^{n}P(A_{i})P(B|A_{i})}$$
![Pasted image 20221106160256.png|450](/img/user/Assets/Pasted%20image%2020221106160256.png)


# אי תלות
__הגדרה:__
$$P(A|B)= P(A)$$
או 

$$P(A\cap B)= P(A)P(B)$$

1) מאורעות זרים הם תמיד בלתי תלויים.
2) אם $A,B$ בלתי תלויים כלומר ההתרחשות של $B$ לא משפיע על $A$ אז גם אי ההתרחשות ולכן אם $A,B$ בלתי תלויים אז גם $A,B^{c}$ בלתי תלויים.

## אי תלות מותנת 
__הגדרה:__

$$P(A\cap B| C)= P(A|C)P(B|C)$$
או

$$P(B|C)P(A|B\cap C)$$

## אי תלות של n מאורעות
נאמר שהמאורעות $A_{1},A_{2},A_{3},\dots ,A_{n}$ בלתי תלויים אם מתקיים 
$$P\bigg (\bigcap_{i\in S}A_{i}\bigg)= \prod_{i\in S}P(A_{i})\ \ \  \text{for every subset S of } \{1,2,3,4,\dots, n \} $$

למשל עבור $A_{1,2,3}$ אי תלות בין שלושתם תתקיים אם :
$$\displaylines{
P(A_{1}\cap A_{2})=P(A_{1})P(A_{2}),\\
P(A_{1}\cap A_{3})= P(A_{1})P(A_{3}), \\
P(A_{2}\cap A_{3})= P(A_{2})P(A_{3}), \\
P(A_{1}\cap A_{2}\cap A_{3})= P(A_{1})P(A_{2})P(A_{3})
}$$

שלושת התנאים הראשונים מבטיחים לנו שכל שתי מאורעות הם בלתי תלויים, התכונה הזאת נקראת  __pairwise independent__ כלומר אי תלות בזוגות. __אי תלות בזוגות לא מראה על אי תלות__


# מנייה, צירופים וחלוקות
מספר תרחישים בהם אנחנו משתמשים בספירה כדי לחשב הסתברות.
1) כאשר מרחב המדגם $\Omega$ הוא מרחב מדגם שווה הסתברות בדיד. כלומר יש בו מספר סופי של תוצאות שלכל אחת הסתברות זהה. במצב זה ההסתברות של מאורע $A$ תהיה
$$P(A)= \frac{|A|}{|\Omega|}$$
2) כאשר נרצה לחשב התסברות של מאורע $A$ עם מספר סופי של תוצאות אפשריות כאשר לכל אחד יש הסתברות $p$ . במצב זה ההסתברות של  מאורע $A$ תהיה

$$p\cdot |A|$$

## עקרונות ספירה
![Pasted image 20221113215020.png|350](/img/user/Assets/Pasted%20image%2020221113215020.png)
__נכליל את עקרונות הספירה כך:__
נניח שיש תהליך שמכיל $r$ שלבים 
1) בשלב הראשון יש $n_{1}$ תוצאות אפשריות.
2) לכל תוצאה אפשרית בשלב הראשון יש $n_{2}$ , תוצאות אפשריות בשלב השני.
3) באופן כללי, לכל רצף של תוצאות אפשריות ב $i-1$ השלבים הראשונים יש $n_{i}$ אפשרויות בשלב ה$i$ .  

### דוגמאות 

1) __מספר הסוגים האפשריים למספר טלפון__ - מספר טלפון הוא בעל 7 ספרות כאשר הספרה הראשונה חייבת להיות שונה מ $0,1$. נוכל לתאר את הבעיה הזאת כתיאור של עץ סדרתי כאשר הרמה ה $i$ מייצגת את הספרה ה$i$ ואפשרותיה. כלומר באופן הזה עבור הספרה הראשונה יש לנו 8 אפשרויות וכל השאר עם 10 ולכן נקבל 
$$8\cdot \underbrace{10\cdot10\cdots 10}_{\text{6 times}} = 8\cdot 10^{6}$$

2) __מספר תתי הקבוצות לקבוצה בגודל $n$__ . נוכל להסתכל על זה כבחירה על כל איבר בקבוצה של האם להכניס אותו לתת הקבוצה או לא. כלומר שישנן $2^{n}$ אפשרויות לבניית תת קבוצה מ$n$ איברים.

## פרמוטציה מסדר k
נרצה לבחור k אובייקטים מתוך אוסף המכיל n אובייקטים שונים ולסדר אותם איכשהו. 

$$n(n-1)(n-2)\cdots(n-k+1)= \prod_{i=1}^{k}n-i+1  \cdot \frac{ (n-k)\cdots 2\cdot1}{(n-k)\cdots 2\cdot1}= \frac{n!}{(n-k)!}$$
במקרה הפרטי ש $k=n$ זה פשוט נקרא פרמוטצייה ונקבל $n!$ אפשרויות.

## צירופים 
רצה לבנות ועדה של $k$ חברי צוות מקבוצה של $n$ אנשים. 

$$\frac{n!}{k!(n-k)!}$$
זה שקול ל מקדם בינומי שזה בעצם בחירה של $k$ מתוך $n$ איברים. כלומר

$$\binom{n}{k}= \frac{n!}{k!(n-k)!} $$



# משתנים מקריים  בדידים

$$p_{X}(x)= P(\{X=x\})= P(\{w\in \Omega\  | \ X(w)=x \})$$
__ניתן גם לסמן $P(X=x)$__ 

__נורמליות:__
$$\sum\limits_{x}p_{X}(x)=1$$

## פונקציות של משתנים רנדומיים בדידים 

$$p_{Y}(y)= \sum\limits_{\{x\ | \ \ g(x)=y\}}p_{X}(x)$$

## תוחלת ושונות

__תוחלת:__

$$E[X]=\sum\limits_{x}xp_{X}(x)$$
$$E[g(X)]= \sum\limits_{x}g(x)\cdot p_{X}(x)$$


__שונות:__

$$\text{var}(X)=E[(X-E[X])^{2}]$$

__סטיית תקן__:

$$\sigma_{X}= \sqrt{var(X)}$$

### תכונות חשובות של תוחלת ושונות 
1) ליניאריות התוחלת:
$$Y=aX+b$$
כאשר $a,b$ הם סקלרים כלומר $Y$ הוא פונקצייה ליניארית על $X$ , יתקיים: 
$$E[Y] = \sum\limits_{x}(ax+b)p_{X}(x)= a \sum\limits_{x}xp_{X}(x)+ b\sum\limits_{x}p_{X}(x) = aE[X]+b$$
2) 
$$var(X)= E[X^{2}]- (E[X])^{2}$$

3) $$X\geq 0 \rightarrow E[X]\geq 0$$
4) עבור $c$ קבוע יתקיים $E[c]= c$ 


$$var(aX+b)= a^{2}var(X)$$

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

__CDF:__

$$F_{geo}(n)= 1- (1-p)^{n}$$

__חוסר זכרון של גיאומטרי:__

$$p_{X-n|X>n}(k)= p_{X}(k)$$
או

$$P(X=n +k | X>n)= P(X=k)$$
> [!info] משתנה מתפלג גיאומטרי שסופר את הכשלונות בלבד
> עד כה מידלנו את ההתפלגות הגיאומטרית ככזאת שסופרת את מספר הניסיונות כולל ההצלחה הראשונה, נוכל בקלות למדל אותו כך שיספור רק את הכשלונות . למשל נוכל פשוט להחסיר מהתוחלת את $1$ כדי לקבל את התוחלת הרצוייה. כלומר ה pmf יהיה פשוט 
> $$p_{X}(k)= (1-p)^{k}p$$ 
> כלומר ספרנו את כל הכשלונות לא כולל ההצלחה. ההשפעה של זה על התוחלת תהיה :
> $$E[X]= \frac{1}{p}-1= \frac{1-p}{p}$$ 
> ההחסרה נובעת מכך שפשוט הורדנו ספירה אחת בניסוי. 
> כמו כן- השונות נשארת אותו דבר כלומר: 
> $$\frac{1-p}{p^{2}}$$

##  פואסון
עם פרמטר $\lambda$ (כאשר $n$ גדול ממש ו$p$ קטן ממש נקבל $\lambda = np$) 
$$\forall_{k\in \mathbb{N}} : p_{X}(k) = e^{-\lambda}\cdot \frac{\lambda^{k}}{k!}$$
$$E[X]=var(X)=\lambda$$



# איחוד PMF של מספר משתנים רנדומיים

$$p_{X,Y}(x,y)= P(X=x, Y=y)$$

עבור $Z=g(X,Y)$:

$$p_{Z}(z)= \sum\limits_{(x,y)\ |\ g(x,y)=z}p_{X,Y}(x,y)$$

$$E[g(X,Y)]= \sum\limits_{(x,y)}g(x,y)p_{X,Y}(x,y)= \sum\limits_{x}\sum\limits_{y}g(x,y)p_{X,Y}(x,y)$$

במקרה הליניארי :

$$E[g(X,Y)]= E[aX+bY+c]= aE[X]+bE[Y]+c$$


__הפונקציות השוליות__ 

  $$p_{X}(x)= \sum\limits_{y}p_{X,Y}(x,y) \ \ \ p_{Y}(y)=\sum\limits_{x}p_{X,Y}(x,y)$$

$$E\left[\sum\limits_{i=1}^{n}a_{i}\cdot X_{i}\right]= \sum\limits_{i=1}^{n}a_{i}\cdot E[X_{i}]$$


## הכללה למספר משתנים אקראיים

$$p_{X,Y,Z}(x,y,z)= P(X=x,Y=y,Z=z)$$

$$p_{X,Y}(x,y)= \sum\limits_{z}p_{X,Y,Z}(x,y,z)$$

$$p_{X}(x)= \sum\limits_{y}\sum\limits_{z}p_{X,Y,Z}(x,y,z)$$

$$E[g(X,Y,Z)]= \sum\limits_{x}\sum\limits_{y}\sum\limits_{z}g(x,y,z)p_{X,Y,Z}(x,y,z)$$

$$E[aX+bY+cZ+d]= aE[x]+bE[Y]+cE[Z]+d$$


# התניות על משתנה רנדומי בדיד

## התנייה של משתנה רנדומי עם מאורע

$$p_{X|A}(x)= P(X=x|A)= \frac{P(\{X=x\}\cap A)}{P(A)}$$

$$P(A)=\sum\limits_{x}P(\{X=x\}\cap A)$$

## התנייה של משתנה רנדומי עם משתנה רנדומי אחר

$$p_{X|Y}(x|y)= \frac{P(X=x, Y=y)}{P(Y=y)}= \frac{p_{X,Y}(x,y)}{p_{Y}(y)}$$


$$p_{X,Y}(x,y)= p_{X|Y}(x|y)\cdot p_{Y}(y)$$

$$p_{X,Y}(x,y)= p_{X}(x)\cdot p_{Y|X}(y|x)$$



## תוחלת מותנת


$$E[X|A]= \sum\limits_{x}x \cdot p_{X|A}(x)$$

$$E[g(x)|A]= \sum\limits_{x}g(x)\cdot p_{X|A}(x)$$

$$E[X|Y=y]= \sum\limits_{x}x\cdot p_{X|Y}(x|y)$$

__התוחלת השלמה__ :
 אם $A_{1},A_{2},A_{3},\dots,A_{n}$ הן מאורעות זרים שמהווים חלוקה של מרחב המדגם וכל אחד מהם עם התסברות גדולה מ0 אזי 
$$E[X]=\sum\limits_{i=1}^{n}P(A_{i})E[X|A_{i}]$$

יתרה מכך, לכל מאורע $B$ שיקיים $\forall_{1\leq i\leq n}: P(A_{i}\cap B)>0$   אזי 

$$E[X|B]=\sum\limits_{i=1}^{n}P(A_{i}|B)E[X|A_{i}\cap B]$$

$$E[X]= \sum\limits_{y}p_{Y}(y)\cdot E[X|Y=y]$$

# אי תלות על משתנה רנדומי בדיד

## אי תלות של משתנה רנדומי עם מאורע

$$P(X=x \cap A) = P(X=x)P(A)= p_{X}(x)P(A)$$

כל עוד $P(A)>0$ , אי תלות היא זהה לתנאי 
$$\forall_{x}: p_{X|A}(x)= p_{X}(x)$$


## אי תלות בין משתנים רנדומים

$$\forall_{x,y}:p_{X,Y}(x,y)=p_{X}(x)p_{Y}(y)$$

## אי תלות מותנת

$$\forall_{x,y}: P(X=x,Y=y | A)= P(X=x|A)P(Y=y|A)$$

## תוחלת ואי תלות

$$E[g(X)h(Y)]=E[g(X)]E[h(Y)]$$

__מסקנה__ 

$$ E[XY]=E[X]E[Y]$$

## שונות ואי תלות

$$var(X+Y)=var(X)+var(Y)$$

## אי תלות של מספר משתנים רנדומים

$$\forall_{x,y,z}: p_{X,Y,Z}(x,y,z)= p_{X}(x)p_{Y}(y)p_{Z}(z)$$

__משפט__ אם $X_{1},\dots,X_{n}$ הם משתנים בלתי תלויים אזי 
$$var\left(\sum\limits_{i=1}^{n} X_{i}\right)= \sum\limits_{i=1}^{n} var(X_{i})$$

## תוחלת ושונות של sample mean 

נגדיר את ה sample mean $S_{n}$ להיות 
$$M_{n}= \frac{\sum\limits_{i=1}^{n}X_{i}}{n}$$
$$E[M_{n}]= \sum\limits_{i=1}^{n} \frac{1}{n}E[X_{i}]= \frac{1}{n}np=p$$

נשים לב שזה משתנה רנדומי שמהווה אסטימצייה טובה לשיעורי ההצבעה בגלל שהתוחלת שלו היא עדיין $p$ שמייצג את שיעור ההצבעה ״האמיתי״ של ביידן. נשים לב שכל עוד $X_{i}$ בלתי תלויים אחד בשני ויש להם תוחלת ושונות שווים אז יתקיים 
$$var(M_{n})= \frac{var(X)}{n}$$



# משתנים רנדומיים - רציף

## PDF 

$$P(X\in B)= \int_{B}f_{X}(x)dx$$
$$P(a\leq X\leq b)= \int\limits_{a}^{b}f_{X}(x)dx$$
__התכונות ש$f_{X}$ צריכה לקיים כדי להיות $PDF$ תקין הן:__
1) $f_{X}(x)\geq 0$ כלומר פונקצייה אי שלילית לכל ערכיה
2) נורמליזציה הסכום של כל ערכי התמונה האפשריים צריך להיות $1$ 
$$\int_{\pm\infty} f_{X}(x)dx = P(-\infty<X< \infty)=1$$

__תוחלת__
$$E[X]=\int\limits_{-\infty}^{\infty} xf_{X}(x)dx$$
## משתנה רנדומי אחיד רציף

$$f_{X}(x)=\begin{cases} \frac{1}{b-a}& x\in[a,b] \\ 0&else\end{cases}$$
$$E[X]= \frac{a+b}{2}$$
$$var(X)= \frac{(b-a)^{2}}{12}$$
__ה CDF של התפלגות אחידה__:

 $$\int_{a}^{x}f_{X}(t)dt= \frac{x-a}{b-a} $$


## משתנה מקרי מעריכי

$$f_{X}(x)= \begin{cases} \lambda e^{-\lambda x} & x\geq 0 \\ 0& else\end{cases}$$

נשים לב שההסתברות של $X$ לעלות על ערך מסויים קטנה באופן מעריכי. עבור $a\geq 0$ נקבל
 $$P(X\geq a)= -e^{-\lambda x}\bigg|_{a}^{\infty}= e^{-\lambda a}$$

![Pasted image 20221218014805.png|350](/img/user/Assets/Pasted%20image%2020221218014805.png)


$$\displaylines{
E[X]= \frac{1}{\lambda}\\ var(X)= \frac{1}{\lambda^{2}}
}$$
__CDF:__

$$F_{exp}(x)=1-e^{-\lambda x} $$

## משתנה מקרי נורמלי

$$f_{X}(x)= \frac{1}{\sqrt{2\pi}\sigma}e^{\frac{-(x-\mu)^{2}}{2\sigma^{2}}}$$
$$E[X]=\mu \ \ \ var(X)=\sigma^{2}$$
_נהוג לסמן משתנה נורמלי $X$ כ $N(\mu,\sigma^{2})$_

### משתנה נורמלי ופונקצייה ליניארית
אם $X$ הוא משתנה נורמלי עם תוחלת $\mu$ ושונות $\sigma^{2}$ נגדיר עבור $a\neq 0$ ו $b$ סקלרים:
$$Y=aX+b$$
אזי, $Y$ הוא משתנה נורמלי ומקיים 
$$E[Y]= a\mu + b \ \ \ \ var(Y)=a^{2}\sigma^{2}$$

### משתנה נורמלי סטנדרטי
משתנה נורמלי $Y$ עם תוחלת 0 ושונות באורך 1 מוגדרת כ __נורמלי סטנדרטי__ . ה CDF שלה מסומן כ $\Phi$ והיא מקיימת

$$\Phi(Y)= P(Y\leq y)= P(Y<y)= \frac{1}{\sqrt{2\pi}}\int_{-\infty}^{y} e^{\frac{-y^{2}}{2}}dt  $$

$$\Phi(-y)=  1-\Phi(y)$$

![Pasted image 20230104022924.png|600](/img/user/Assets/Pasted%20image%2020230104022924.png)

__נירמול משתנה נורמלי__:
_בהינתן משתנה מקרי $X$ עם תוחלת $\mu$ ושונות $\sigma^{2}$, נבצע תהליך בין 2 שלבים:_ 
א) ״נירמול״ $X$ והגדרת משתנה נורמלי סטנדרטי $Y= \frac{x-\mu}{\sigma}$ 
ב) חישוב ה CDF באופן הבא

$$P(X\leq x)= P\left(\frac{X-\mu}{\sigma}\leq \frac{x-\mu}{\sigma}\right)= P\left(Y\leq \frac{x-\mu}{\sigma}\right)= \Phi\left(\frac{x-\mu}{\sigma}\right)$$


# CDF פונקציית ההתפלגות המצטברת

$$F_{X}(x)= P(X\leq x)= \begin{cases} \sum\limits_{k\leq x}p_{X}(k) & \text{if X is discrete} \\  \\
\int\limits_{-\infty}^{x}f_{X}(t)dt&\text{if X is continuous} \end{cases}$$

![Pasted image 20221218025342.png|350](/img/user/Assets/Pasted%20image%2020221218025342.png)

## תכונות
1) $F_{X}$ היא מונוטונית עולה כלומר $x\leq y \rightarrow F_{X}(x)\leq F_{X}(y)$
2) $F_{X}(x)$ שואפת ל $0$ כאשר $x\to -\infty$ ושואפת ל $1$ כאשר $x\to\infty$ .
3)  אם $X$ הוא משתנה רציף אז $F_{X}(x)$ היא פונקצייה רציפה של $x$  כלומר רציפה מימין. ויתקיים 

$$\frac{d}{dx} F_{X}(x)= f_{X}(x)$$
5)  אם $X$ הוא בדיד ומקבל ערכים טבעיים, נוכל לחלץ את ה CDF וה PMF על ידי סכימת ההפרשים כלומר 
$$p_{X}(k)= P(X\leq k)- P(X\leq k-1)= F_{X}(k)-F_{X}(k-1)$$
5)  אם $X$ הוא רציף, ה $PDF$ וה $CDF$ מתקבלים אחד מהשני על ידי גזירה ואינטגרצייה.
6) $P(a\leq X\leq b)= F_{X}(b)-F_{X}(a)$ 

__הרבה פעמים נרצה להשתמש ב CDF כדי לחשב את ה PMF או ה PDF, את ה PDF נוכל לחשב על ידי חישוב CDF וגזירה ובאופן דומה על ה PMF על ידי הנוסחה הנ״ל__ 

## משתנה מקרי מעורב
מודלים הסתברותיים לרוב מערבים משתנים רנדומים שמערבים בתוכם מעין מיקס של בדיד $Y$ ורציף כלשהו $Z$ בכך אנחנו מתכוונים שהערך של $X$ מורכב מהחוקים ההסתברותיים הפועלים תחת $Y$ עם הסתברות $p$ ומהחוקים ההסתברותיים הפועלים תחת $Z$ עם הסתברות משלימה של $1-p$ 
_הרעיון הוא שיש אזורים נקודתיים במרחב שלנו שיש בהם נקודות אי רציפות ואז במקום שההסתברות של ערך בודד תהיה 0 במקרה רציף יהיה שם ערך מסויים וסכום כל ההסתברויות של הנקודות האלה יהיה המשלים של שאר השטח._

במצב זה $X$ ייקרא __משתנה מקרי מעורב__. וה CDF שלו ניתן לחישוב באמצעות נוסחת ההסתברות השלמה 

$$F_{X}(x)= P(X\leq x)= pP(Y\leq x)+(1-p)P(Z\leq x)=pF_{Y}(x)+(1-p)F_{Z}(x)$$

באמצעות משפט התוחלת השלמה נקבל 

$$E[X]= pE[Y]+ (1-p)E[Z]$$


# איחוד PDF ו CDF של מספר משתנים רנדומיים

## JOINT PDF 

$$P((X,Y)\in B) = \int\limits_{(x,y)\in B}\int f_{XY}(x,y)dxdy$$

$$P(a\leq X\leq b \cap c\leq Y\leq d) = \int_{c}^{d}\int_{a}^{b} f_{X,Y}(x,y)dxdy$$
__השוליות:__

$$f_{X}(x) = \int_{-\infty}^{\infty} f_{X,Y}(x,y)dy$$
באופן דומה נקבל 
$$f_{Y}(y)= \int_{-\infty}^{\infty} f_{X,Y}(x,y)dx$$




### PDF משותף אחיד 

לכל תת קבוצה $S$ במישור הדו מימדי, הPDF המשותף האחיד על S יהיה
$$f_{XY}(x,y) =\begin{cases}  \frac{1}{\text{area(S)}} & (x,y)\in S \\ 0 & else\end{cases}$$

ולכל $A\subset S$ ההסתברות ש$(X,Y)$ נמצא ב $A$ תהיה 

$$P((X,Y)\in A) = \frac{\text{area(A)}}{\text{area(S)}} $$

## CDF משותף

$$F_{X,Y}(x,y)= P(X\leq x, Y\leq y)= \int_{-\infty}^{x}\int_{-\infty}^{y}f_{X,Y}(s,t)dtds$$


## תוחלת משותפת

$$E[g(X,Y)]=\int\limits_{-\infty}^{\infty}\int\limits_{-\infty}^{\infty} g(x,y)f_{X,Y}(x,y)dxdy$$
$$ E[aX+bY+c]= aE[X]+bE[Y]+c$$

## הכללה למספר משתנים מקריים

ה PDF המשותף של שלושה משתנים מקריים $X,Y,Z$ מוגדר באופן דומה למקרה של שתי משתנים מקריים רציפים:

$$P((X,Y,Z)\in B)= \underset{(x,y,z)\in B}{\int\int\int}f_{X,Y,Z}(x,y,z)dxdydz$$
באופן דומה למקרה של שתי משנים נוכל לחלץ את הפונקציות השוליות

$$\displaylines{
f_{X,Y}(x,y)
=\int\limits_{-\infty}^{\infty}f_{X,Y,Z}(x,y,z)dz \\
\\
f_{X}(x)=\int\limits_{-\infty}^{\infty}\int\limits_{-\infty}^{\infty} f_{X,Y,Z}(x,y,z)dydz
}$$

התוחלת תקיים:

$$E[g(X,Y,Z)]=\int\limits_{-\infty}^{\infty}\int\limits_{-\infty}^{\infty}\int\limits_{-\infty}^{\infty} g(x,y,z)f_{X,Y,Z}(x,y,z)dxdydz$$

והמקרה הפרטי הליניארי:

$$E[g(X,Y,Z)]=E[aX+bY+cZ]=aE[X]+bE[Y]+cE[x]$$

כמובן שנוכל להכליל את הנוסחה של התוחלת הנ״ל באופן ברור עבור $X_{1},X_{2},\dots,X_{n}$ משתנים מקריים רציפים:

$$E\left[\sum\limits_{i=1}^{n} a_{i}X_{i}\right]=\sum\limits_{i=1}^{n}a_{i}E[X_{i}]$$



# אי תלות בין משתנים מקריים רציפים

$$f_{X,Y}(x,y)= f_{X}(x)f_{Y}(y)$$

$$F_{X,Y}(x,y)= P(X\leq x, Y\leq y)= P(X\leq x) P(Y\leq y)= F_{X}(x)F_{Y}(y)$$

$$E[g(X)h(Y)]= E[g(X)]E[h(Y)]$$


אם $X,Y,Z$ בלתי תלויים אז גם כל $f(X),g(Y),h(Z)$ שנבחר יהיו בלתי תלויים. באופן דומה , כל 2 משתנים מקריים מהצורה $g(X,Y),h(Z)$ הם בלתי תלויים.
__למרות זאת__ ברוב המקרים שתי פונקציות $g(X,Y),h(Y,Z)$ כן יהיו תלויות אחד בשני בגלל ששתיהן מושפעות מ $Y$ . 

# קונבולוצייה 

$$\displaylines{
p_{Z}(z)= P(X+Y=z) = \sum\limits_{x} p_{X}(x)p_{Y}(z-x)
}$$

$$f_{Z}(z)= \int_{-\infty}^{\infty} f_{X,Z}(x,z)dx=\int_{-\infty}^{\infty}f_{X}(x)f_{Y}(z-x)dx$$


# שונות משותפת ומתאם

$$cov(X,Y)=E\big[(X-E[X])(Y-E[Y])\big] $$

$$cov(X,Y)= E[XY]-E[X]E[Y]$$

### תכונות השונות המשותפת

$$\displaylines{
cov(X,Y)=cov(Y,X)\\
cov(X,X)= var(X) \\
cov(X,aY+b)= a\cdot cov(X,Y) \\
cov(X,Y+Z)= cov(X,Y)+cov(X,Z)\\
cov(X+Y,Z)= cov(X,Z)+cov(Y,Z) \\
cov(X,a)=0\\
cov(X,aX+b)=a\cdot var(X)
}$$

כמו כן בהינתן ש $X,Y$ בלתי תלויים אנחנו יודעים ש $E[XY]=E[X]E[Y]$ ולכן יתקיים $cov(X,Y)=0$ . 

## מקדם המתאם של פירסון

$$\rho(X,Y)= \frac{cov(X,Y)}{\sqrt{var(X)var(Y)}}$$

## השונות של סכום של משתנים מקריים

$$var(X_{1}+X_{2})=var(X_{1})+var(X_{2})+2cov(X_{1}+X_{2})$$

$$var\left(\sum\limits_{i=1}^{n} X_{i}\right)= \sum\limits_{i=1}^{n}var(X_{i})+\sum\limits_{\{(i,j) \ | \ i\neq j\}}cov(X_{i},X_{j})$$


# משפטי גבול

## אי שוויון מרקוב (Markov inequality)

$$P(X\geq a)\leq \frac{E[X]}{a} \ \ \ \ \text{for all a>0}$$

## אי שוויון צבישב (Chebyshev inequality)

יהי $X$ משתנה מקרי עם תוחלת $\mu$ ושונות $\sigma^{2}$ אזי

$$P(|X-\mu|\geq c)\leq \frac{\sigma^{2}}{c^{2}} \ \ \ \ \text{for all c>0}$$

### משלים של אי שיוויון צ׳בישב

$$P(|X-E[X]|\leq c)= 1- P(|X-E[X]|\geq c)\geq 1- \frac{var(X)}{c^{2}}$$


## החוק החלש של המספרים הגדולים

$$M_{n}= \frac{X_{1}+\dots+ X_{n}}{n}= \frac{S_{n}}{n}$$

$$E[M_{n}]= \mu \ \ \ var(M_{n})= \frac{\sigma^{2}}{n}$$

__הגדרה__

הי $X_{1},X_{2}\dots$ סדרה של משתנים בלתי תלויים שמתפלגים באופן זהה עם תוחלת $\mu$ . אזי לכל $\epsilon>0$ נקבל

$$P(|M_{n}-\mu|\geq\epsilon)= P\left(\left| \frac{X_{1}+\dots+ X_{n}}{n} -\mu\right|\geq \epsilon\right)\overset{n\to\infty}{\to}0$$


## משפט הגבול המרכזי
$$S_{n}= X_{1}+\dots + X_{n}=nM_{n}$$

$$E[S_{n}]= nE[M_{n}]=n\mu$$

$$var(S_{n})= var(X_{1})+var(X_{2})+\dots + var(X_{n})= n\sigma ^{2}$$


נגדיר:

$$Z_{n}= \frac{S_{n}-n\mu}{\sigma\sqrt{n}}= \frac{X_{1}+\dots+X_{n}-n\mu}{\sigma\sqrt{n}}$$

__הוא בעל תוחלת 0 ושונות 1__ 

__נגדיר את משפט הגבול המרכזי באופן הבא:__ 
$$\lim_{n\to\infty}P(Z_{n}\leq z)= \phi(z)$$

### קירובים מבוססים משפט הגבול המרכזי

__משפט קירוב נורמלי מבוסס משפט הגבול המרכזי__:
יהי $S_{n}=X_{1}+X_{2}+\dots+X_{n}$ נוכל להתייחס להסתברות $P(S_{n}\leq c)$ בקירוב נורמלי באופן הבא:

1. חשב את $n\mu$ והשונות $n\sigma^{2}$ של $S_{n}$ .
2. חשב את הערך המנורמל $z=\frac{c-n\mu}{\sigma\sqrt{n}}$  .
3. תשתמש בקירוב: 

$$P(S\leq c)\approx \Phi(z)$$


