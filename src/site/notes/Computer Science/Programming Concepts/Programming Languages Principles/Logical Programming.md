---
{"dateCreated":"2024-03-05 01:22","tags":["programming_language"],"pageDirection":"rtl","dg-publish":true,"permalink":"/computer-science/programming-concepts/programming-languages-principles/logical-programming/","dgPassFrontmatter":true}
---

# תכנות לוגי
ראינו ב [[Computer Science/Programming Concepts/Programming Languages Principles/OCaml\|OCaml]] עקרונות של תכנות פונקציונלי שהוא מופע של [פרדיגמת](https://en.wikipedia.org/wiki/Programming_paradigm) declarative programming שמבוסס הצהרות וגם את [[Computer Science/Programming Concepts/Programming Languages Principles/Semantics#שפת while\| while]] שהיא שפת התכנות הפרוצדורלית תחת פרדיגמת [imperative programming](https://en.wikipedia.org/wiki/Imperative_programming) הבסיסית ביותר. 

![Pasted image 20240306001822.png|350](/img/user/Assets/Pasted%20image%2020240306001822.png)

ישנם מופעים נוספים תחת הפרדיגמות השונות ואחד מהם הוא __התכנות הלוגי__ תחת פרדיגמת ה [declarative programming](https://en.wikipedia.org/wiki/Declarative_programming)

>[!note] מויקיפדיה
>![Pasted image 20240305013043.png](/img/user/Assets/Pasted%20image%2020240305013043.png)
## Prolog
פרולוג היא שפת תכנות לוגית שפותחה במקור לכתיבת יישומי בינה מלאכותית. השפה היא [[Computer Science/Computational Models/Turing Machine#התזה\|שלמה טיורינג]], כלומר ניתן לממש באמצעותה כל מה שאפשר לממש בשפות התכנות הנפוצות. שמה נגזר מצירוף המילים PROgramming LOGic.

### ביטויים
1) מספרים
2) אטומים - ביטויים המתחילים באות תחילית lower case או בגרשיים `elephant, xYZ, a_123, ’Another pint please` . (נקראים גם atomic term)
3) משתנים- מתחילים באות גדולה
4) ביטויי קריאה (compound term) - מורכב מאטום ומספר ביטויים למשל f(t1,t2...) .
	* אטומים וביטויי קריאה נקראים __predicates__
	* ביטויי קריאה בלי משתנים נקראים ground terms
5) functors - זוג סגור של שמכיל $\text{<autom, num of variables>}$ למשל <a,5> זה אטום שמכיל 5 ארגומנטים. נוכל להשתמש בkeyword השמור בשפה functor כדי לבדוק האם ביטויי קריאה שהגדרנו מקיים את ״תנאי״ הפנקטור למשל :

```Prolog
functor(a(x,y,z,w,v),a,5) -> will return true
```

6) רשימות (נתמקד בהמשך)

### פסוקיות
פסוקית ב prolog היא אחת מהצורות הבאות
1) עובדות
![Pasted image 20240305222302.png|200](/img/user/Assets/Pasted%20image%2020240305222302.png)
2) חוקים- נכתבים בסינתקס  cute(x):-cat(x) המשמעות של :- זה if או is implied by.

_סינתקס:_

``` Prolog
rule_name(object1, object2, ...) :- fact/rule(object1,
 object2, ...)
Suppose a clause is like :
P :- Q;R.
This can also be written as
P :- Q.
P :- R.

If one clause is like :
P :- Q,R;S,T,U.

Is understood as
P :- (Q,R);(S,T,U).
Or can also be written as:
P :- Q,R.
P :- S,T,U.
```
התו ״,״ מייצג את הפסוק and .

לדוגמה: 
happy(lili) :- dances(lili).
hungry(tom) :- search_for_food(tom).
friends(jack, bili) :- lovesCricket(jack), lovesCricket(bili).
goToPlay(ryan) :- isClosed(school), free(ryan).

3) פרדיקטים- יהי f/n פנקטור. הפרדיקט שלו הוא אוסף הפסוקיות ש f מופיע בראשון. 
 
__תוכנית-__ בפרולוג, תוכנית היא רצף של פסוקיות. נסתכל על תוכנית שמגדירה עובדות על ה״עולם״ וחוקים 

```prolog
parent(pam,bob)
parent(tom,bob)
parent(tom,liz)
parent(bob,ann)
parent(bob,pat)
parent(pat,jim)


*if X is parent of Z then he is ancestor of Z*
ancestor(X,Z) :- parent(X,Z) 

*if X is parent of Y and Y is ancestor of Z then X is ancestor of Z*
ancestor(X,Z) :- parent(X,Y), ancestor (Y,Z)
```

כעת נראה מה קורה כשננסה לבדוק האם חוקים מתקיימים למשל נקבל ש parent(pam,bob) יחזיר true ו ancestor(pam,ann) יהיה query של החוק הראשון שהוא נתקל בו בקוד (__הסדר משנה__) ולכן הוא ״מופע״ של החוק הראשון וצריך לשאול האם parent(pam,ann) מתקיים, מאחר והתשובה היא לא הוא ילך לחוק השני וינסה למצוא התאמה עבור השאילתה הזו. המקרים היחידים שצריך לבדוק הם parent(pam,bob) כי רק במקרה הזה X מתקים לערך השאילתה ובמקרה הזה מתקיים גם ש ancestor(bob,ann) הוא true כי parent(bob,ann) מתקיים.


נגדיר את מה שעשינו בצורה פורמלית-

__הגדרה:__
מופע של פסוקית c הוא פסוקית $c'$ המתקבלת מ c על ידי החלפת המשתנים בביטויים כלשהם.

__הגדרה:__ G (השאלה) נכונה ב P (תוכנית) אם יש C (חוק) ב P ומופע שלה I (שאילתה) כך ש
	1) הראש של I זהה ל G
	2) הגוף של I נכון ב P 
עבור: a :- b1,...,bn   מתקיים ש a הוא הראש והשאר זה הגוף.


 הסינתקס לשאילתה הוא מהצורה- $?- bigger(elephant, monkey).$ נוכל להחליף את אחד הערכים במשתנה ולקבל אילו ערכים נוכל לשים עבור המשתנה הזה מהעולם כדי שהשאילתה תהיה נכונה

  ![Pasted image 20240306000230.png|300](/img/user/Assets/Pasted%20image%2020240306000230.png)
אם אין יותר פתרונות יוחזר NO.

__שיוויון אריתמטי ו שיוויון סינתקסטי__
== - מייצג שיוויון סינתקסטי 
=:= - מייצג שיוויון אריתמטי 

למשל - 3+4 =:= 2+5 יחזיר true אבל אם נשים == נקבל false. 

ניתן לבדוק שיווין [[Computer Science/Programming Concepts/Programming Languages Principles/Type Inference\|unifiable]] עם האופרטור = . למשל $3+4=3+X$ יחזיר X=4 שכן אם נחליף X ב 4 נקבל ביטויים שווים ברמת הסינתקס.

### לולאות
נדגים בנייה של לולאה באמצעות הגדרות רקרוסיביות:

```Prolog
sumto(1,1)
sumto(N,S) :- N>1 , M is N-1, sumto (M,R), S is R+N
```

S הוא הסכום של כל המספרים מ 1 עד N . הגדרנו חוק בסיס שה sum(1,1) כלומר הסכום של 1 הוא 1. 
לאחר מכן הגדרנו חוק על כל מקרה אחר שמחייב ש N>1 וגם עבור משתנה זמני M=N-1 יתקיים שהסכום של כל המספרים מ1 עד ל M יהיה R ולבסוף צריך שיתקיים S=R+N . בעצם הגדרנו כאן לולאה באמצעות כללי רקורסיה.

למשל עבור sum(4,10) נקבל true וכפי שאמרנו נוכל לקבל את הערך עצמו אם נשאיר את S כנעלם למשל sum(4,S) ״יחזיר״ S=10. 

>[!info] לולאה אינסופית
>נוכל לייצר לולאה אינסופית באמצעות הסינתקס P:-P מההסבר שלנו על איך הקומפיילר של השפה מחפש פתרון נקבל שנתקע

### רשימות
דבר ראשון מגדירים $\text{member(X,[X|L])}$ כלומר תמיד יתקיים הכלל ש X הוא חבר ברשימה שהוא האיבר הראשון שלה. נגדיר גם את כלל $\text{member(X,[Y|L]) :- member(X,L)}$ כלומר X הוא חבר ברשימה ש Y הוא האיבר הראשון שלה אם הוא חבר בתת הרשימה L. 

נקבל למשל ש $\text{member(1,[1,2,3,4])}$ יחזיר true ואבל $\text{member(5,[1,2,3,4,])}$ יחזיר false.

נראה כעת קוד דוגמה שממיין מערך בסדר הפוך:

``` Prolog
simplsort(X,Y) :- permut(X,Y), sorted(Y).

sorted([]).
sorted([_]).
sorted([A,B | X]) :- A =< B, sorted([B | X]).

permut([], []).
permut(X, [A | Y]) :- concat(X1, [A | X2], X), concat(X1, X2, X3), permut(X3, Y).

concat([], Y , Y).
concat([A|X],Y, [A|Z]) :- concat(X,Y,Z)
```

הכלל simplesort מבקש ש sorted(Y) וגם permut(X,Y) יתקיימו. 

sorted מוגדר כ true על רשימה ריקה ואחרת מוגדר ממויין אם כל איבר A שבא לפני B במערך יקיים ש $A\leq B$ .
permut מוגדר כ true על שני מערכים ריקים אחרת הוא רוצה לבדוק האם מערך X הוא פרמוטציה של $[A|Y]$ על ידי כך שהוא מחייב ש A נמצא איפשהו ב X והוא בודק את זה על ידי הכלל concat שבודק האם שרשור של שני מערכים נותן את המערך השלישי. 