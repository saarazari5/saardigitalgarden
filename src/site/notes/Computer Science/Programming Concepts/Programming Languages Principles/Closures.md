---
{"dateCreated":"2024-03-17 01:31","tags":["programming_language"],"pageDirection":"rtl","dg-publish":true,"permalink":"/computer-science/programming-concepts/programming-languages-principles/closures/","dgPassFrontmatter":true}
---

# Expression Evaluation 

כדי להבין כיצד עובד האבלואציה של ביטויים ב[[Computer Science/Programming Concepts/Programming Languages Principles/OCaml\|OCaml]]. לאחר מכן נבין כיצד נכנסים closures לתמונה הזאת בשביל לאפשר תכונות מסוימות.

נסתכל על קטע הקוד הפשוט הבא
```OCaml
let x = 1 in 
	if true then 2 else x
```

אנחנו יודעים שהמנגנון של פירוש הסמנטיקה של OCaml הוא מבוסס על [[Computer Science/Programming Concepts/Programming Languages Principles/Lambda calculus#Substitution\|substitutions]] כלומר הinterpreter יראה את קטע הקוד הבא ויבצע החלפה של כל המקומות שבהם מופיע x בתוך קטע הקוד בערך שלו 1 ונקבל if true then 2 else 1. המנגנון הזה טוב אבל הוא לא מושלם, שכן היינו יכולים לדעת מניתוח קוד בצורה אחרת ש if true תמיד מתקיים ופשוט להחזיר 2 על כל הביטוי. בדוגמה הזאת זה נראה משהו זניח, אבל אם זה היה קורה בתוך קוד שיש לו לולאה מאוד ארוכה המנגנון הזה היה יכול לעזור מאוד מבחינת יעילות. 

נרצה לממש אינטרפטציה עצלה בדומה ל[[Computer Science/Programming Concepts/Programming Languages Principles/Semantics#שפת while\|שפת while]]. כלומר מבוססת מצבים. 

__סביבה:__
היא רשימה מהצורה $(x_{1},v_{1})\dots (x_{n}, v_{n})$ כך שכל $x_{i}$ הוא משתנה ו $v_{i}$ הוא ערך. כמו כן, $(x_{1},v_{1})$ הוא ראש הרשימה . ראש הרשימה מסומן ב $E(x)$.

__הגדים:__ 
יחס ה evaluation מסומן כך $E\vdash e\to v$ ומוגדר לפי כללי ההיסק שמיד נגדיר. 

נסמן- $(x:v):E$ היא הסביבה המתקבלת מ E על ידי השמת v ל x. 

## כללי היסק 
1) const - $\dfrac{}{E\vdash v\to v}$
2) var - $\dfrac{E(x)= v}{E\vdash x\to v}$
3) let- $\dfrac{E\vdash e_{1}\to v_{1} \ \ (x:v_{1}):E\vdash e_{2}\to v}{E\vdash \text{let x = e1 in e2}\to v}$

_דוגמה:_
נראה ש let x = 3 in x $\to$ 3 .

$$\dfrac{\dfrac{const}{[]\vdash 3\to 3} \ \ \ \dfrac{var}{(x:3):[]\vdash x\to 3}}{[]\vdash \text{let x =3 in x}\to 3}$$

איך לוקחים את הנ״ל לעולם של פונקציות.

כדי לעשות זאת ננסה להוסיף כלל חדש של application

$$App-\dfrac{E\vdash e_{1} \to \text{ func }x\to e \ \ E\vdash e_{2}\to v_{2} \ \ (x:v_{2})::E \vdash e\to v}{E\vdash e_{1}e_{2}\to v}$$

במילים, כדי שההפעלה של $e_{1}$ על $e_{2}$ תתן את v צריך ש $e_{1}$ יהיה פונקציה שמקבל קלט x ומבצעת את הביטוי e. כמו כן צריך ש $e_{2}$ הקלט לפונקציה יהיה הערך $v_{2}$ ולבסוף צריך שאם x ממופה ל $v_{2}$ אז זה יגרום ש $e$ ימופה לv.

נשמע הגיוני, אבל נראה ש __הכלל הזה לא עובד__ באמצעות הניסיון שלנו לבצע את הevaluation על הביטוי הבא

$$\displaylines{\text{let x=1 in let f = fun y }\to \text{x}\\ \text{ in let x= 2 in f 0}\to ?}$$

קל לראות שיש כאן בעיה של scopes. שכן איזה x חוזר מהפונקציה f, החיצוני או הפנימי? אם נריץ את הקוד הזה בOCaml נקבל שהפלט לדבר הזה הוא 1. 

כעת ננתח לפי הכלל החדש APP שהגדרנו , אנחנו נראה שבסביבה הריקה הביטוי הנ״ל (נסמנו e) יוערך ל 2. לשם הנוחות נסמן את הביטוי שאחרי ה in הראשון כ $e'$ .

$$Let:\dfrac{\dfrac{const}{[]\vdash 1\to 1} \ \ \dfrac{\dfrac{const}{E\vdash \text{fun y}\to x\to \text{ fun y }\to x} \ \ \dfrac{\dfrac{const }{E'\vdash 2\to 2} \ \ \dfrac{}{\color{red}E''\vdash f 0\to 2}}{E'\vdash \text{ let x =2 in f0}\to 2}}{\underbrace{(x:1)::[]}_{E}\vdash \text{let f = fun y }\to \text{x in e''}\to 2}}{[]\vdash \text{let x = 1 in e'}\to 2}$$

כמעט סיימנו, הגענו ל $E''\vdash f 0 \to 2$ כאשר-  $E''=(x:2):: E'=(x:2)::(f:\text{ fun y}\to x):: E$ נראה מה קורה כשנרצה להראות שההפעלה של f על 0 תיתן 2 באמצעות כללי app

$$\dfrac{\dfrac{VAR}{E''\vdash f\to \text{ fun y }\to x} \ \ \dfrac{CONS}{E''\vdash 0\to 0} \ \ \dfrac{VAR}{(y:0):: E''\vdash x \to 2}}{E''\vdash f 0\to 2}$$

ניתן לראות שבסוף קיבלנו ש x נגזר ל2 בגלל הכלל VAR כיוון ש $E'$ מכיל את הכלל (x:2) שנכנס אחרי (x:1).

__מה הבעיה בכלל הזה?__
ההבדל העיקרי הוא ש OCaml עובד לפי static scoping כלומר ההפנייה למשתנה היא תמיד ה top level scope ובהגדרה שלנו השתמשנו ב dynamic scoping כלומר לוקחים את הערך העדכני ביותר מהסביבה (E(x)).

נגדיר את זה-
__סקופ דינמי :__ הערכים של המשתנים בגוף של הפונקציה מקושר לפי הסביבה שבה היא רצה
__סקופ סטטי :__  הערכים של המשתנים בגוף של הפונקציה מקושר לפי הסביבה שבה היא הוגדרה.

כדי לטפל בבעייה הזאת, נרצה להגדיר מבנה שונה מהסביבה שלא מתייחס לפונקציה כערך ומאפשר לנו לדעת לאן ממופים כל המשתנים הלא קשורים שמופיעים בה מראש, לפני הפעלת הפונקציה. 

>[!note] 
>כמעט ואין שפות היום שמשתמשות בסקופ דינמי וגם אלו מאפשרות גם סקופ סטטי למשל lisp, racket ו perl

## סגור- closure
כאן נכנס הclosure לתמונה. נגדיר אותו כזוג סדור מהצורה <env,exp> כך ש 
* env - הסביבה 
* exp- ביטוי מהצורה $\text{ fun x}\to \text{ e}$ .

כעת נוכל לנסח את הכלל APP מחדש

$$\dfrac{E \vdash e_{1}\to <E', \text{ fun x}\to e> \ \ \ E \vdash e_{2}\to v_{2} \ \ \ (x:v_{2})::E'\vdash e\to v}{E \vdash e_{1}e_{2}\to v}$$

אז כעת השוני העיקרי הוא שאנחנו עובדים עם $E'$ הסביבה שבה הפונקציה הוגדרה ולא עם הסביבה E. 

כדי לוודא שניתן לגזור פונקציה רק בסביבה שבה היא הוגדרה נשתמש בכלל FUN :

$$\dfrac{}{E\vdash \text{ fun x}\to e\Rightarrow <E,\text{ fun x}\to e>}$$

זה בעצם כלל שדומה ל const רק שהפעם זה אומר שבמקום להגיד שזה ממופע לעצמו זה ממופע לזוג שהגדרנו וככה מחזיקים את הסביבה שהייתה בזמן שהפונקציה הוגדרה.

אם נחזור כעת, לדוגמה שלנו נוכל להראות ש e ממופה ל1 כמו ב OCaml

![Pasted image 20240317131202.png](/img/user/Assets/Pasted%20image%2020240317131202.png)

## closures ו recursion
נזכיר שהsyntax של פונקציה רקורסיבית הוא מהצורה
$$\text{ let rec f = fun x}\to \dots$$

הבעיה בהגדרה ריקורסיבית עם מה שעשינו זה שאנחנו צריכים להגדיר לאן כל ערך בתוך הפונקציה f ממופה בסביבה שבה הפונקציה הוגדרה, הבעיה עם זה היא שf מכיל את עצמו וזה עלול מיפוי מהצורה:

![Pasted image 20240317134935.png|250](/img/user/Assets/Pasted%20image%2020240317134935.png)

כלל ההיסק של let rec יהיה

$$\dfrac{E'\vdash e_{2}\to v}{E \vdash \text{let rec f x = }e_{1} \text{ in }e_{2}\to v}$$

בעצם אומרים שהביטוי התחתון בסביבה E ימופה ל v אם $e_{2}$ ימופה ל v בסביבה $E'$. כאשר
$E' =(f:<E', \text{fun x}\to e_{1}>)::E$ . גם כאן יש הגדרה רקורסיבית מוזרה אבל נראה למה זה עובד בהמשך.

ניקח את הדוגמה הבאה
$$[]\vdash\text{let rec f x = }\underbrace{\text{if x}\leq 1\text{ then 1 else  x}\cdot \text{f(x-1) }}_{e} \text{in f 2}\Rightarrow 2$$

נסמן את הביטוי עד ה in ב e. כמו כן נגדיר את שתי הסביבות הבאות

$$\displaylines{
E' = (f:<E, \text{ fun x}\to e>)::[]\\
E'' = (x:2)::E'
}$$


אם כן נתחיל לפתח את כללי הגזירה כמה שאפשר עם הנתונים שיש ברשותנו

$$\dfrac{\dfrac{\dfrac{var}{E'\vdash f\Rightarrow <E' , \text{fun x }\to e>} \ \dfrac{const}{E'\vdash 2\Rightarrow 2} \ \ \dfrac{}{\color{red}(x:2)::E'\vdash e\Rightarrow 2}}{E'\vdash \text{f 2} \Rightarrow 2}}{[]\vdash \text{ let rec f x = e in f 2}\Rightarrow 2}$$

כדי לפתוח את הכלל המסומן באדום, נצטרך להגדיר מספר כללים נוספים שכן e הוא ביטוי מהצורה if else. כמו כן יש לנו בפנים ביטויים אריתמטיים.

![Pasted image 20240319001903.png|350](/img/user/Assets/Pasted%20image%2020240319001903.png)

 כעת עם הכללים האלה נוכל להמשיך לפתח את הגזירה

$$\dfrac{\dfrac{\dfrac{var}{E''\vdash x\Rightarrow 2}\dfrac {cons}{E''\vdash 1\Rightarrow 1}\dfrac{}{2>1}}{E''\vdash x\leq 1\Rightarrow fls} \ \ \dfrac{\dfrac{var}{E''\vdash x\Rightarrow 2} \dfrac{}{\color{red}E''\vdash f(x-1)\Rightarrow 1}  \ \ \dfrac{}{1\cdot 2 = 2}}{E''\vdash x \cdot f(x-1)\Rightarrow 2}}{(x:2)::E'\vdash e\Rightarrow 2}$$

כעת נפתח גם הביטוי המסומן 

$$\dfrac{\dfrac{var}{E''\vdash f \Rightarrow <E',\text{fun x}\to e>} \ \dfrac{\dfrac{var}{E''\vdash x \Rightarrow 2} \ \dfrac{cons}{E''\vdash 1\Rightarrow 1} \ \dfrac{}{2-1 \Rightarrow 1}}{E''\vdash x-1\Rightarrow 1} \ \dfrac{}{\color{red}(x:1)::E'\vdash e \Rightarrow 1}}{E''\vdash f(x-1)\Rightarrow 1}$$

ונפתח את הביטוי האחרון (באדום)

$$\dfrac{\dfrac{\dfrac{var}{E'''\vdash x\Rightarrow 1} \ \dfrac{cons}{E'''\vdash 1\Rightarrow 1} \ \dfrac{}{1\leq 1}}{E'''\vdash x\leq 1 \Rightarrow tru} \ \dfrac{cons}{E''' \vdash 1\Rightarrow 1}}{(x:1):: E' \vdash e \Rightarrow 1}$$

__וסיימנו__.

==כדי לסכם:==
1. ראינו שכדי לבצע evaluation של ביטויים באוקמל חייבים להשתמש בסביבה.
2. כדי שהסקופ יהיה סטטי צריך להשתמש ב closure.
3. בשפות אחרות closure יכול להיות מוגדר אחרת למשל ב java הוא אובייקט עם הרבה שדות ומתודה אחת אבל הרעיון הוא אותו רעיון.


## OCaml Scopes 
כדי לחשב את let x = e1 in e2 בסביבה כלשהי env
	1) __Evaluate__ - נחשב את $e_{1}$ לערך $v_{1}$ בסביבה env , $env::e_{1}\to v$ .
	2) __Extend__ - הרחבה של הסביבה כך שהיא מכילה את ההצמדה של המשתנה $x$ ל $v_{1}$ . $env'=env[x\mapsto v_{1}]$ .
	3) __Evaluate__ - נחשב שוב פעם , את $e_{2}$ לערך $v_{2}$ תחת הסביבה $env'$ 
	4) מחזירים את $v_{2}$ 


נדגים על הביטוי let f= fun x -> x in f 0.
הסביבה היא $env_{0}=[]$ .
1) הביטוי fun x -> x הוא כבר ערך , הפונקציה עצמה. 
2) נגדיר $env_{1}=env_{0}[f\mapsto fun \ x\to x]=[f\mapsto fun \ x \to x]$ 
3) נחשב את הביטוי הפנימי f 0 תחת $env_{1}$.
	1) נחליף את f להיות הפונקציה ואת 0 ל 0 
	2) $env_{2} = env_{1}[x \mapsto 0]$ 
	3) תחת הסביבה הזאת קל לחשב ש $x \Rightarrow 0$. 

בעצם יצרנו ״סביבת עבודה״ או closure שבה התוצאה x ממופה לערך מסויים. נדגים על קוד קצת יותר מורכב כדי להבין מה הכוונה. 

```OCaml
let x = 1 in
	let f = fun y -> x in 
		let x = 2 in
			f 0
```

הפונקציה הזאת באוקמל מחזירה 1 (בגלל הסקופ הסטטי שדיברנו עליו, כלומר הערך של x בזמן הגדרת הפונקציה הוא הקובע)

אם ניקח את קוד c השקול נקבל 

```c
{
	int x = 1
	{
		int f(int y)
		{
			return x;
		}
		{
			int x = 2;
			printf("%d", f(0));
		}
	}
}
```

בקוד הזה, יוחזר 2 כי הscope הוא דינמי ומה שחשוב זה מה הערך של x בזמן הקריאה לפונקציה.

_כפי שאמרנו אנחנו נגדיר את הפונקציות שלנו כclosure כלומר מכילות גם את קטע הקוד של הפונקציה וגם את הסביבה שנשמרת בזמן הגדרת הפונקציה._

__דוגמה 1__
```OCaml
let x = 1 in
	let f = fun y -> x in
		let x = 2 in
			let z = f 0 in z
```

שורה 2 יוצרת את הclosure : $<(x:1)::[], \text{fun y}\to \text{x}>$ ולכן בזמן יצירת הפונקציה זאת הסביבה שלנו. 
בשורה 4 קוראים ל closure עם הארגומנט 0 אבל הראנו בכללי הגזירה שמסתכלים על הסביבה שבה הפונקציה הוגדרה ולכן z=1.

__דוגמה 2__

```OCaml
let x = 1 in
	let f y = x + y in
		let x = 3 in
			let y = 4 in
			 let z = f(x+y) in z 
```

בשורה 2 יצרנו closure של $<\text{ f y}\to x , [x\mapsto 1]>$ ולכן הסביבה שלנו היא $(x:1)$. בשורה 5 קוראים ל closure של שורה 2 כאשר הסביבה היא $[x\mapsto 3,y\mapsto 4]$ ולכן הארגומנט שנשלח הוא ביחס לסביבה הזאת כלומר 7. בגלל שההפעלה של הפונקציה תלויה בסביבה שבה הפונקציה הוגדרה נקבל ש z=7+1=8 .

_בסקופ דינמי- היינו מחשבים את z לפי הערך של x כאשר הפונקציה נקראה ולכן היינו מקבלים $3+7=10$_ 

>[!note] scope chain
>נשים לב שאם שפה מסויימת לא מוצאת בscope מסויים משתנה היא בדר״כ כלל תלך לscope שמעליה כדי לחפש את ההצהרה , רק אם היא לא תמצא בscope הגלובלי תזרק שגיאה. התנהגות כזאת נקראת scope chain search

