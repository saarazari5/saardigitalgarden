---
{"dateCreated":"2022-12-05 21:41","tags":["probability","computer_science"],"pageDirection":"rtl","dg-publish":true,"permalink":"/computer-science/probability/joint-pmf/","dgPassFrontmatter":true}
---


# איחוד PMF של מספר משתנים רנדומיים
מודלים הסתברותיים לרוב כוללים מספר משתנים רנדומיים. לדוגמה, בהקשר של אבחון רפואי, התוצאה של כמה בדיקות יכולה להיות משמעותית, או למשל במודל תקשורת, עומס העבודה של כמה ראוטרים יכול לעניין אותנו.
זה נותן מטיבציה לשקול הסתברויות של מאורעות שמכילים מספר משתנים רנדומיים בו זמנית.

יהי שתי [[Computer Science/Probability/Discrete Random Variables\|משתנים מקריים בדידים]]  $X,Y$ שקשורים לאותו ניסוי. ההתסברויות של הערכים ש $X,Y$ יכולים לקחת נתפסים על ידי ה $joint-PMF$ של $X,Y$ . נסמנו $p_{X,Y}$ 
אם ניקח זוג $(x,y)$ שמייצג שתי ערכים של $X,Y$ בהתאמה, אזי מסת ההסתברות של $(x,y)$ היא ההתסברות של המאורע 
$$\{X=x, Y=y\}= \{X=x \cap Y=y\}= \{w\in\Omega \ | \  X(w)=x\wedge Y(w) =y\}$$
אם כן, יתקיים 
$$p_{X,Y}(x,y)= P(X=x, Y=y)$$

איחוד pmf קובע את ההסתברות של כל מאורע שיכול להיות מתואר על ידי המשתנים הרנדומים $X,Y$ . למשל: בהינתן קבוצה $A$ שמכילה את כל הזוגות $(x,y)$ עם תכונות מסויימות אזי:

$$P((X,Y)\in A)= \sum\limits_{(x,y)\in A}p_{X,Y}(x,y)$$
 
  __כאשר $A=\Omega$ הסכום הנ״ל הוא 1__ 
  
  יתרה מכך, נוכל לחשב את פונקציית מסך ההסתברות של $X$ ושל $Y$ בנפרד על ידי הנוסחה:
  $$p_{X}(x)= \sum\limits_{y}p_{X,Y}(x,y) \ \ \ p_{Y}(y)=\sum\limits_{x}p_{X,Y}(x,y)$$

הוכחה: 

$$p_{X}(x)= P(X=x)= \sum\limits_{y}P(X=x, Y=y)= \sum\limits_{y}p_{X,Y}(x,y)$$

המעבר השני נכון בגלל שהקבוצה $\{X=x\}$ היא האיחוד של כל __הקבוצות הזרות__ 

$$\bigcup_{y} \{X=x, Y=y\}$$
הקבוצות זרות כי משתנה רנדומי הוא פונקצייה ולכן לא ייתכן שתוצאה כלשהי בניסוי תוביל אותי לכמה $y$.  כמו כן חשוב לזכור שמשתנה רדנומי ממדל את כל מרחב המדגם מעצם העובדה שמרחב המדגם הוא התחום שלו, כלומר כל מאורע שנפעיל עליו את הפונקצייה שהיא המשתנה הרנדומי תביא לערך כלשהו ולכן תמיד יהיה חיתוך עבור משתנים אקראיים בדידים. נזכיר שמ [[Computer Science/Probability/The Complete Probability Theorem and Bayes' Law\|ההסתברות השלמה]] אנחנו יודעים איך לחשב הסתברות לפי החיתוך שלו עם קבוצות זרות שמשלימות למרחב המדגם שזה בידיוק כל המאורעות $\{Y=y\}$ עבור כל ה $y$ האפשריים.

מתייחסים ל $p_{X}$ ו $p_{Y}$ כ $PMF$ השולי, כדי להבדיל אותם מה PMF  המאוחד.

לרוב משתמשים ב __שיטה טבלאית__ כדי לחשב את הpmf השולי לדוגמה
![Pasted image 20221206134604.png|450](/img/user/Assets/Pasted%20image%2020221206134604.png)
נרשום בטבלה את פונקציית מסת ההתסברות המאוחדת לפי ערכים ונסכום לפי עמודה או שורה רלוונטית כדי לקבל פונקציית מסת הסתברות של ערך מסויים.
למשל לפי הטבלה יתקיים 
$$p_{X}(2)= \frac{1}{20}+ \frac{2}{20}+ \frac{2}{20}+ \frac{1}{20}= \frac{6}{20}$$


## פונקציות על מספר משתנים רנדומיים
כאשר יש מספר משתנים רנדומיים שמעניינים  אותנו, זה אפשרי לייצר משתנה רנדומי חדש על ידי הגדרת פונקצייה שלוקחת בחשבון ערכים ממספר משתנים רנדומיים. למשל  עבור פונקצייה $Z=g(X,Y)$ של $X,Y$ משתנים רנדומיים מוגדר לנו $Z$ משתנה רנדומי חדש. את פונקציית מסת ההסתברות pmf שלו ניתן לחשב באופן הבא

$$p_{Z}(z)= \sum\limits_{(x,y)\ |\ g(x,y)=z}p_{X,Y}(x,y)$$


יתרה מכך ה[[Computer Science/Probability/Discrete Random Variables#תוחלת\|תוחלת]]  של פונקצייה על מספר משתנים רנדומיים תיראה כך 
$$E[g(X,Y)]= \sum\limits_{(x,y)}g(x,y)p_{X,Y}(x,y)= \sum\limits_{x}\sum\limits_{y}g(x,y)p_{X,Y}(x,y)$$
נוכל אם כן להרחיב את החוק הנ״ל למקרה פרטי של ליניאריות התוחלת באופן הבא:
$$g(X,Y)= aX+bY+c \ \ | \ \ a,b,c\in\mathbb{R}$$
אזי
$$E[g(X,Y)]= E[aX+bY+c]= aE[X]+bE[Y]+c$$

_הוכחה-_ 

$$\displaylines{
E[g(X,Y)]= \sum\limits_{x}\sum\limits_{y}g(x,y)p_{X,Y}(x,y)= \sum\limits_{x}\sum\limits_{y}(ax+by+c)p_{X,Y }(x,y) =\\
a\sum\limits_{x}\sum\limits_{y}xp_{X,Y}(x,y)+ b\sum\limits_{x}\sum\limits_{y}yp_{X,Y}(x,y)+ c\sum\limits_{x}\sum\limits_{y}p_{X,Y}(x,y)= \\
a\sum\limits_{x} x \sum\limits_{y}p_{X,Y}(x,y)+a\sum\limits_{y} y \sum\limits_{x}p_{X,Y}(x,y)+c = \\
a\sum\limits_{x}xp_{X}(x)+ b\sum\limits_{y}yp_{Y}(y)+c= aE[X]+bE[Y]+c
}$$


__נשים לב להכללה הבאה:__
$$E\left[\sum\limits_{i=1}^{n}X_{i}\right]= \sum\limits_{i=1}^{n}E[X_{i}]$$

ונוכל להכליל זאת אף יותר 
$$E\left[\sum\limits_{i=1}^{n}a_{i}\cdot X_{i}\right]= \sum\limits_{i=1}^{n}a_{i}\cdot E[X_{i}]$$

## הכללה למספר משתנים אקראיים
ה PMF המשותף של שלושה משתנים רנדומים $X,Y,Z$ מוגדר באופן הבא
$$p_{X,Y,Z}(x,y,z)= P(X=x,Y=y,Z=z)$$
וכמובן שבאופן דומה ל PMF מאוחד של שתי משתנים, ה PMF השולי יהיה
$$p_{X,Y}(x,y)= \sum\limits_{z}p_{X,Y,Z}(x,y,z)$$
וגם 
$$p_{X}(x)= \sum\limits_{y}\sum\limits_{z}p_{X,Y,Z}(x,y,z)$$


התוחלת של פונקצייה המופעלת על שלושת הפרמטרים תהיה
$$E[g(X,Y,Z)]= \sum\limits_{x}\sum\limits_{y}\sum\limits_{z}g(x,y,z)p_{X,Y,Z}(x,y,z)$$
והמקרה הפרטי שבו $g$ ליניארי יקיים 
$$E[aX+bY+cZ+d]= aE[x]+bE[Y]+cE[Z]+d$$

## התוחלת של משתנה בינומי
מומלץ לרפרף כאן [[Computer Science/Probability/Discrete Random Variables#משתנה/התפלגות בינומי\|התפלגות בינומית]] לפני תחילת הבעיה
נתחיל מדוגמה, נניח שיש לנו כתה עם 300 סטודנטים ולכל סטודנט יש הסתברות של $\frac{1}{3}$ לקבל 100 במבחן, ללא תלול באף סטודנט אחר.
מהי התוחלת של $X$ כאשר הוא משתנה רנדומי שמתאר מספר הסטודנטים שקיבלו 100.

כדי לענות על השאלה נגדיר את המשתנה הרנדומי אינדיקטור הבא
$$X_{i}=\begin{cases}  1 & \text{the i-th student got 100}\\ \\
0&else
\end{cases}$$
כלומר יש לנו עבור $n$ סטודנטים $X_{1,2,3,4,\dots,n}$ משתני אינדיקטור עם הסתברות משותפת של $p$ . נשים לב שההסתברות היא גם התוחלת במקרה הזה.
יתקיים ש 
$$X=\sum\limits_{i=1}^{n} X_{i}$$
וזה ייתן לנו את מספר הסטודנטים שקיבלו 100. 
כלומר תיארנו את הבעיה באמצעות מידול של $n$ ניסויים בלתי תלויים כלומר התפלגות בינומית עם המשתנים $n,p$ . מליניאריות התוחלת נקבל 
$$E[X]= E\left[\sum\limits_{i=1}^{n} X_{i} \right]= \sum\limits_{i=1}^{n} E[X_{i}]$$
בהצבת $n=300$ ו $p=\frac{1}{3}$ נקבל

$$E[X]= 300\cdot \frac{1}{3}=100$$
כלומר במקרה המוכלל של הניסוי בלי הצבה נקבל $E[X]= n\cdot p$


## בעיית הכובעים 
נניח ש $n$ אנשים זורקים את הכובע שלהם בקופסה וכל אחד מוציא כובע מהקופסא באקראי, כאשר כל קובע נבחר על ידי בן אדם אחד בלבד וההסתברות לקבלת כובע הינה שווה לכולם.
נרצה לחשב את התוחלת של $X$ המשתנה הרדנומי המייצג את מספר האנשים שקיבלו את הכובע שלהם בחזרה.
נקבל אם כן בדומה לדוגמה הקודמת שאם נגדיר $n$ משתני אינדיקטור כלומר $X_{i}$ מייצג דגל שקובע האם האיש ה $i$ קיבל את כובעו בחזרה או לא כאשר 
$$E[X_{i}]= P(X_{i})= \frac{1}{n}$$
סך הכל נקבל 
$$E[X]= E\left[\sum\limits_{i=1}^{n} X_{i} \right]= \sum\limits_{i=1}^{n} E[X_{i}]= n\cdot \frac{1}{n}= 1$$
