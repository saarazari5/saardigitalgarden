---
{"dateCreated":"2022-11-05 15:51","tags":["calculus","computer_science"],"pageDirection":"rtl","dg-publish":true,"permalink":"/computer-science/calculus/function-queues/","dgPassFrontmatter":true}
---


# טורים של פונקציות 
__הגדרה__ $\sum\limits_{n=1}^{\infty} f_{n}$ נקראת טור של פונקציות. כמו כן, $S_{n}(x)=\sum\limits_{k=1}^{n}f_{k}(x)$ נקראת סדרה הסכומים החלקיים.

נאמר ש __טור הפונקציות__ מתכנס נקודתית/ במ״ש אם סדרת הסכומים החלקיים שלה מתכנסת נקודתית / במ״ש. והפונקצייה הגבולית של שניהם תהיה שווה. כמו כן, __תחום ההתכנסות__ של שניהם יהיה אותו תחום התכנסות.
(הפונקצייה הגבולית מסומנת כטור עצמו).

### מציאת  תחום התכנסות של טור פונקציות 
מתייחסים ל $x$ כקבוע, ונקבל טור מספרים , כעת נוכל להשתמש בכלים שאנחנו מכירים מאינפי 1 למשל:

$$\sum\limits_{n=1}^{\infty} \frac{x^{3}}{n}=x^{3}\cdot\sum\limits_{n=1}^{\infty} \frac{1}{n}$$
אנחנו יודעים מאינפי 1 שזה טור מתבדר ולכן לכל $x$ הטור יתבדר..


### התכנסות במ״ש של טורי פונקציות
בסופו של דבר כל טור פונקציות הוא מקרה פרטי של סדרה ולכן שיטות הבדיקה זהות..
#### מבחן ה M של ווירשטראס
אם לכל $n\in\mathbb{N}$ ולכל $x$ השייך לקטע שבו מוגדרות טור הפונקציות מתקיים :
$$|f_{n}(x)|\leq a_{n}$$
וגם $\sum\limits_{n=1}^{\infty}a_{n}$ מתכנס, אזי: $\sum\limits_{n=1}^{\infty}f_{n}(x)$ מתכנס במ״ש.

### גבולות במ״שׁ של טורי פונקציות 
1) אם כל $f_{n}$ רציפה ב $I$ ואם יש התכנסות במ״ש ב$I$ אזי, הפונקצייה הגבולית $\sum\limits f_{n}$ רציפה ב $I$.
2) אם $f_{n}$ אינטגרבילית לכל $n$ ב $[a,b]$ ואם הטור מתכנס במ״שׁ בקטע זה אז גם הפונקצייה הגבולית $\sum\limits f_{n}$ אינטגרבילית בקטע ואפשר לבצע אינטגרצייה __איבר איבר__ כלומר [[Computer Science/Calculus/The Integral\|The Integral]] של הסכום שווה לסכום של האינטגרל.
$$\int\limits_{a}^{b}\sum\limits_{n=1}^{\infty}f_{n}=\sum\limits_{n=1}^{\infty}\int_{a}^{b}f_{n}$$
3) אם $\sum\limits f_{n}$ מתכנס ב $I$ (לא חייב בכל הקטע) וטור הנגזרות $\sum\limits f^{\prime}_{n}$  מתכנס במ״ש ב $I$ . אז גם הפונקצייה הגבולית $\sum\limits f_{n}$ מתכנס במ״שׁ ב $I$ ויתרה מכך היא גזירה וניתן לגזור __איבר איבר__ כלומר 

$$\bigg(\sum\limits f_{n}\bigg)^{\prime}=\sum\limits(f_{n})^{\prime}$$
__משפט__ 
$$\forall_{|q|<1}: \sum\limits_{n=0}^{\infty}q^{n}= \frac{1}{1-q}$$
$$\forall_{|q|<1}: \sum\limits_{n=1}^{\infty}q^{n}=\sum\limits_{n=0}^{\infty}q^{n} -1 = \frac{1}{1-q}-1= \frac{q}{1-q} $$

##### אנקדוטה- פונקציה רציפה שאינה גזירה באף נקודה
פונקציית ויירשטראס : 
$$w(x)=\sum\limits_{k=1}^{\infty}\frac{\cos\pi b^{k}x}{a^{k}}$$
זאת פונקצייה רציפה ב $\mathbb{R}$ ואינה גזירה באף נקודה.

## טורי חזקות 
__טור חזקות__ סביב $a$ הוא טור פונקציות מהצורה $\sum\limits_{n=0}^{\infty}a_{n}(x-a)^{n}$ . היתרון בטורי חזקות הוא שניתן למצוא את תחום ההתכנסות לפי נוסחה..

__הטור חזקות הכי קלאסי שאנחנו יודעים לחשב אותו מפורשות הוא הטור ההנדסי__ .

__משפט רדיוס ההתכנסות__ 
קיים מספר $R\geq 0$ או $R=\infty$  כך שטור חזקות מתכנס לכל $x$ שמקיים $|x-a|<R$ ומתבדר אחרת.. $R$ מייצג את רדיוס ההתכנסות, באמצעותו ובאמצעות המרכז של הטור אפשר לדעת את תחום ההתכנסות.. 
תחום ההתכנסות יהיה בוודאות $(R-a,R+a)$  . את הקצוות צריך לבדוק ידני..


__איך מחשבים את הרדיוס? נוסחת קושי-הדמר__ 

1) $$R=\frac{1}{\lim\limits_{n\to\infty}\sqrt[n]{|a_{n}|}}$$
2) $$R= \frac{1}{\lim\limits_{n\to\infty}\big|\frac{a_{n+1}}{a_{n}}\big|}$$
__בתנאי שהגבול במכנה קיים במובן הרחב__ . כמו כן נשים לב שאם המכנס שואף ל 0 רדיוס ההתכנסות הוא אינסופי כלומר בכל הממשיים, ואם שואף ל אינסוף אז הרדיוס הוא 0. כמו כן נשים לב שאם הגבול לא קיים זה לא אומר שאין רדיוס..

#### התכונות של טורי חזקות
1) אם רדיוס ההתכנסות הוא $R$ אז הטור חזקות מתכנס בהחלט בתחום ההתכנסות .
2) __משפט אבל__ - הטור מתכנס במ״ש בכל קטע סגור שמוכל בתחום ההתכנסות.
3) הפונקצייה המתארת את טור החזקות היא פונקצייה רציפה בתחום ההתכנסות.
4) מותר לבצע אינטגרצייה וגזירה איבר איבר בכל הקטע _הפתוח_ של תחום ההתכנסות.
__נשים לב, רדיוס ההתכנסות של האינטגרל והנגזרת הוא אותו רדיוס, פרט לקצוות (אם רדיוס ההתכנסות הוא קטע פתוח אז זה לא משנה, רלוונטי אם רדיוס ההתכנסות הוא קטע סגור), בפרט בגזירה רדיוס ההתכנסות יכול רק לקטון ובאינטגרצייה יכולים רק לגדול__. (אחרי הצבה מחשבים עם כלים של אינפי 1 את התכנסות או התבדרות הטור).

__מסקנה ממשפט אבל__ : 
אם יש טור חזקות שאנחנו יודעים את הסכום שלו בקטע פתוח, אם יש __התכנסות__ בקצוות אז הסכום הוא נכון גם לקצוות הקטע. הסיבה לכך היא שאם יש התכנסות בקצוות בטור חזקות אז זה בפרט התכנסות במ״ש.
ההוכחה נובעת משיווין של פונקציות רציפות.. לא אפרט את ההוכחה כאן.


## טורי טיילור מקלורן 
### פולינומים 
פולינום הוא פונקציה מהצורה $p(x)=\sum\limits_{k=0}^{d}a_{k}x^{k}$ כש$a_{k}$ הם מספרים ממשיים כמו כן אם $a_{n}$ שונה מ$0$ אז $d$ מוגדרת כדרגת הפולינום.


__כללי הגזירה של פולינום__ מראים ש 
$$\displaylines{
p^{\prime}(x)=\sum\limits_{k=1}^{d}ka_{k}x^{k-1}\\
p^{\prime\prime}(x)=\sum\limits_{k=2}^{d}k(k-1)a_{k}x^{k-2}
}$$
ובאופן כללי לכל $0\leq n\leq d$ 
$$p^{(n)}(x)=\sum\limits_{k=n}^{d}k(k-1)(k-2)\dots(k-n+1)a_{k} x^{k-n}$$
ואם $n>d$ נקבל את פולינום ה $0$ .

משהו מעניין שאפשר להסיק מכללי הגזירה האלו היא שאפשר להשיג את המקדם של הפולינום ה $n$-י על ידי הצבת $x=0$ כלומר :
$$p^{(n)}(0)=n!\cdot a_{n}$$ (בכל מקרה שהוא לא $k=n$ נקבל $0$ ורק במקרה שהם שווים נקבל $0^0$ שזה 1)

כלומר סדרת המקדמים $a_{0},a_{1},a_{2},\dots ,a_{d}$ היא בעצם הסדרה $\frac{1}{0!}p(0), \frac{1}{1!}p^{\prime}(0)\dots, \frac{1}{d!}p^{(d)}(0)$ 

__למה__ אם $p$ הוא פולינום אז הפונקצייה הקדומה שלו בממשיים היא פולינום.

__מסקנה__ נניח ש $f$ היא פונקצייה גזירה $d$ פעמים וש $f^{(d+1)} =0$ אז $f$ היא פולינום והיא נתונה על ידי הנוסחה 
$$f(x)=\sum\limits_{k=0}^{d}\frac{f^{(k)}(0)}{k!}x^{k}$$
__הוכחה__ הפונקצייה $f^{(d+1)}$ הוא פולינום ה $0$ 
### פולינומי מקלורן
 תהי $f$ פונקציה הגזירה $n$ פעמים ב $0$ . __פולינום מקלורן__ מסדר $n$ של $f$ הוא הפולינום $P_{n}$ הבא
 $$P_{n}(x)=\sum\limits_{k=0}^{n}\frac{f^{(k)}(0)}{k!}x^{k}$$
דוגמה: 
עבור $f(x)=e^{x}$ נקבל שלכל $n$  : $f^{n}(x)=e^{x}$  ובהצבת $0$ נקבל $1$. לכן פולינום מקלורן מסדר $n$ יהיה
$$P_{n}(x)=1+x+ \frac{x^{2}}{2!}+\dots+ \frac{x^{n}}{n!}=\sum\limits_{k=0}^{n} \frac{x^{k}}{k!} $$
המסקנה היא שניתן לקרב פונקציות שגזירות $n$ פעמים לפולינום מהסוג הזה כיוון שהראנו.

### טורי טיילור ומקלורן 
טור טיילור הוא טור חזקות שהסס״ח שלו הם פולינומי טיילור $P_{n}$ של הפונקצייה. (סביב $0$ זה נקרא טורי מקלורן).  

נשאלת השאלה, מתי פונקצייה מסויימת שווה לטור טיילור/פולינום טיילור שלה בקטע מסויים? 
התשובה לשאלה הזאת מתחלקת לשניים 
* ראשית, צריך להראות שהפונקצייה גזירה אינסוף פעמים בישביל לבנות לה בכלל טור טיילור כזה.
* השיוויון יתקיים אם  כל הנגזרות חסומות באותו קבוע לכל $x$ בקטע המתאר את תחום ההתכנסות.

$$\forall_{n\in\mathbb{N},x\in I} |f^{(n)}(x)|\leq M$$

### טורי טיילור ידועים 
* $$\forall_{x\in(-1,1)}:\frac{1}{1-x}=\sum\limits_{n=0}^{\infty}x^{n}$$
* $$\forall_{x\in\mathbb{R}}: \sin(x)=\sum\limits_{n=0}^{\infty}\frac{(-1)^{n}}{(2n+1)!}x^{2n+1}$$
* $$\forall_{x\in\mathbb{R}}: \cos(x)=\sum\limits_{n=0}^{\infty}\frac{(-1)^{n}}{(2n)!}x^{2n}$$
* $$e^{x}=\sum\limits_{n=0}^{\infty} \frac{x^{n}}{n!}$$
* $$\arctan(x)=\sum\limits_{n=0}^{\infty}\frac{(-1)^{n}}{2n+1}x^{2n+1}$$

##### פעולות שאפשר לעשות על טורים ידועים
* הצבה - עלולה לשנות את תחום ההתכנסות
* גיזרה ואינטגרצייה איבר איבר (כי מדובר בטור חזקות)
* זהויות טריגו
* כפל בסקלר


שימוש בטורים ידועים והפעולות הנ״ל מאפשר לנו להגיע לשיוויונות בין פונקציות אחרות לטורים אחרים בלי הצורך להוכיח את הכל.

### הערכת השארית של טור טיילור 
$$f(x)=\sum\limits_{n=0}^{\infty}\frac{f^{(n)}(a)}{n!}(x-a)^{n}=\sum\limits_{n=0}^{N}\frac{f^{(n)}(a)}{n!}(x-a)^{n}+\sum\limits_{n=N+1}^{\infty}\frac{f^{(n)}(a)}{n!}(x-a)^{n}$$
נרצה לבחור $N$ שיאפשר לנו לקבל הערכה עד דיוק מסויים של טור טיילור באמצעות $P_{N}$ כלומר פולינום טיילור מסדר $N$. 

__סימון__ שארית טיילור היא בעצם $\sum\limits_{n=N+1}^{\infty}\frac{f^{(n)}(a)}{n!}(x-a)^{n}$ והיא מסומנת כ $R_{n}$ והיא מוגדרת להיות 
$$R_{n}=f-P_{n}$$ כלומר הפונקצייה פחות הפולינום מהסדר שבחרנו..

כמו כן המטרה שלנו היא שהשארית תהיה כמה שיותר קרובה ל 0 בערכה המוחלט , נסמן $|R_{n}|$ _כשגיאה_ . ככל שהשגיאה יותר קרובה ל $0$ ככה הפולינום טיילור יותר קרוב בערכו לטור.

__יש הרבה תאורייה מסביב לנושא הזה, כמובן שכיוון שלא התמקדנו בה לאורך הקורס לא אפרט אותה בסיכום זה__ .

__משפט שארית לגרנאז׳__ 
תהי $f$ מוגדרת בקטע $I$ וגזירה בו $n+1$ פעמים סביב $x_{0}$. לכל $x$ שנבחר בקטע נוכל לבנות סביבו פולינום וטור טיילור כך ש 
$$\exists_{c\in (x,x_{0})}:R_{n}(x)=\frac{f^{(n+1)}(c)}{(n+1)!}(x-x_{0})^{n+1}$$
נוכל להעזר במשפט זה כדי לחסום את השארית ב ערך מוחלט בהנתן $x,x_{0}$ שנבחר.

__דוגמה__
 נחשב את $sin(0.1)$ בשגיאה שלא תעלה על $0.001$ 
נגדיר $$f(x)=\sin(x)$$ ו נסמן את $x=0.1$ 
כעת נרצה לבחור את נקודת המרכז של הטור, הרבה פעמים נרצה לבחור נקודה שאנחנו יודעים שקיים טור טיילור סביבה, או נקודה שקרובה ל$x$ שבחרנו. במקרה הזה נוח יהיה לקחת את $0$ .

טור טיילור של סינוס סביב $0$ ייראה מהצורה
$$\sin (x)=x- \frac{x^{3}}{3!}+ \frac{x^{5}}{5!}-\dots$$
כעת, 
$$|R_{n}(x)|=\bigg|\frac{f^{n+1}(c)}{(n+1)!}x^{n+1}\bigg|\leq\frac{(0.1)^{n+1}}{(n+1)!}$$
החסימה הזאת נובעת מכך שאנחנו יודעים שהנגזרת של פונקציית הsin תמיד חסומה ב1 לכל $n$ .

כעת נשאר רק להציב $n=2$ שעבורו החסם שלנו יהיה הכי קרוב מלמטה לשגיאה הרצויה .

כעת נשאר לחשב את פולינום טיילור מדרגה 2 ונקבל את ״ערך״ הפונקצייה בשגיאה הרצויה.

### הערכת השגיאה על טור לייבניץ 
בטור לייבניץ יותר קל לנו לעמוד את השגיאה כי מתקיים תמיד 
$$|R_{n}|\leq |a_{n+1}|$$ בניגוד למקרה הקודם של טורים כללים שהיינו צריכים לבטא את השארית באמצעות משפט לגרנאז כדי לחסום אותו, כאן אנחנו יכולים לחסום אותו ישירות. 

__נשים לב__, יכולים לשאול גם את השאלה ההפוכה, בהינתן $N$ מסויים להעריך את השגיאה של פולינום טיילור ביחס ל פונקצייה עצמה.. זאת שאלה יותר פשוטה לטעמי שכן, היא מאפשרת חישוב ישיר.

