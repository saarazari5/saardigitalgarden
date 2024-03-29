---
{"dateCreated":"2024-01-04 09:46","tags":["programming_language"],"pageDirection":"rtl","dg-publish":true,"permalink":"/computer-science/programming-concepts/programming-languages-principles/o-caml/","dgPassFrontmatter":true}
---

# OCaml
OCaml זאת שפת תכנות נוחה ל[[Computer Science/Programming Concepts/Programming Languages Principles/Functional Programming\|תכנות פונקציונלי]] אם כי היא תומכת בתכנות מונחה עצמים גם כן.
OCaml ותכנות פונקציונלי ככלל ישמש את הצעד הראשונה לכניסה אל מאחורי הקלעים של שפות תכנות, פרדיגמות וקונבנציות שמשתמשים בהם כאשר ניגשים לעבוד עם שפות תכנות או לפתח אחת כזו. 

## חמשת הזוויות של שפות תכנות
לפני שנכנס ל OCaml עצמה. חשוב להכיר את חמשת האספקטים של שפות תכנות:

__syntax__ - מילות מפתח, הגדרות, סימונים. כיצד שפת התכנות בנויה ברמה הבסיסית ביותר.
__semantics__ - החוקים הלא כתובים שקובעים את האופן בו התוכנה תרוץ ותתנהג. כשמדברים על סמנטיקה מתייחסים לשני אספקטים. _הדינמית_ שמתארת את ההתנהגות בזמן ריצה של התוכנית. ו _הסטטית_ שקשורה לבדיקות בזמן קומפילצייה.
__idioms__- קונבנציות שבהם מפתחים מנוסים משתמשים כדי לתאר התנהגות מסוימת באמצעות השפה.
__libraries__- מהם הכלים שמפתחים אחרים פיתחו שעומדים ברשותך. יכול לעזור בבחירת שפה מתאימה עבור פרוייקט מסויים.
__Tools__- קומפיילר, אינטרפטר, IDE וכו׳

## The OCaml Toplevel
הTopLevel הוא כמו מחשבון או CLI ל-OCaml. זה דומה ל-JShell עבור Java, או הinterpreter האינטראקטיבי של Python. הוא שימושי לניסיון של פיסות קוד קטנות מבלי לטרוח ולהפעיל את מהדר OCaml. אבל אל תסתמכו על זה יותר מדי, כי יצירה, קומפילציה ובדיקה של תוכניות גדולות ידרשו כלים חזקים יותר. שפות אחרות יקראו לרמה העליונה REPL, אשר מייצג read-eval-print-loop: הוא קורא קלט מתכנת, מעריך אותו, מדפיס את התוצאה ואז חוזר. המומלץ ביותר עבור OCaml הוא `utop`

## Expressions, Values and Types
בתכנות פונקציונלי ובפרט ב OCaml. היחידה הבסיסית ביותר של תוכנית נקראת expression. 
שפות תכנות שכאלה נקראות [expression oriented](https://en.wikipedia.org/wiki/Expression-oriented_programming_language). בעולם ה[[Imperative Programming\|Imperative Programming]] שפות כאלה נקראת command oriented.

תוכניות בOCaml הן בעצם expressions, כלומר, הצהרות בקוד לא מתארות פעולות שצריך לבצע על data כלשהו כמו בשפות אחרות אלא הקומפיילר מבצע evaluation בזמן ריצה של הביטויים שבסופם מתקבל ערך (value).
כל expression מורכב מ: 
1) syntax
2) semantics:
	* type - checking (static semantics)
	* evaluation rules (dynamic semantics)


אפשר להסתכל על value כ expression שלא צריך חישוב דינמי
![Screenshot 2024-01-06 at 22.12.13.png|350](/img/user/Assets/Screenshot%202024-01-06%20at%2022.12.13.png)

נסתכל על דוגמה פשוטה:
```OCaml
# 42;; 
- : int = 42
```

כאשר נרשום ב `utop` את השורה `;;42` נקבל את הפלט הנ״ל. ננתח אותו מימין לשמאל
א) 42 זה הvalue
ב) `int` זה הטיפוס של הvalue.
ג) כיוון שלא הגדרנו שום שם לערך הוא יקבל symbol דיפולטי `-`

דוגמאות נוספות:
```OCaml
# "Every expression has a type";;
- : string = "Every expression has a type"

# 2 * 21;;
- : int = 42

# int_of_float;;
- : float -> int = <fun>

# int_of_float (3.14159 *. 2.0);;
- : int = 6

# fun x -> x * x;;
- : int -> int = <fun>

# print_endline;;
- : string -> unit = <fun>

# print_endline "Hello!";;
Hello!
- : unit
```

>[!note] הבחנה
>האריתמטיקה של float ב OCaml שונה בסינתקס שלה מהאריתמטיקה של integers, בניגוד לשפות אחרות אם נרצ לבצע אריתמטיקה בין floats נצטרך להוסיף נקודה אחרי הפעולה למשל `.*` יהווה כפל בין מספרים עם נקודה עשרונית. ההחלטה לעשות זאת היא design choice של המפתחים כדי למנוע overload של אופטורים ולמנוע דו משמעות.

הטיפוס של ביטוי לפני evaluation והטיפוס של הערך הנוצר לאחר חישוב הביטוי הם זהים. בצורה זו הקומפיילר לא צריך לבדוק בזמן ריצה את הטיפוסים. ב OCaml הקומפיילר משמיט את המידע על הטיפוס כך שהוא כלל לא זמין בזמן ריצה. הקונספט הזה נקרא [subject reduction](https://en.wikipedia.org/wiki/Subject_reduction)

>[!note] Type inference and annotation
> הקומפיילר של OCaml מבצע הבחנה לטיפוסים (type inference) כלומר אנחנו לא מחוייבים להצהיר על הטיפוס מפורשות (למשל כמו ב Java). החישוב של הטיפוסים נעשים בזמן קמפול ותהיה שגיאה אם לא ניתן להעריך מהו הטיפוס בזמן קימפול. גם ב OCaml אפשר להצהיר על הטיפוס מפורשות, באמצעות הסינטקס $(e:t)$  כאשר צד שמאל זה הביטוי והימני זה הטיפוס

## Let Definition
היכולת להצמיד value ל name (בשפות אחרות, הצהרת משתנה). 
חשוב לשים לב שכאן, בגלל הסיבה שאנחנו עובדים בתכנות פונקציונלי, ברגע שהצהרתי על משתנה ערכו נשאר קבוע. ב OCaml מצהירים באמצעות `let` למשל `let x = 42`.

אם נריץ את זה ב utop נקבל `val x : int = 42`. כלומר הפלט הוא val שערכו הוא 42 והשם שלו הוא $x$. למעשה definitions שכתובות בtop level מוגדרות כ __global definitions__. 

__definition__ אינם expressions אלא מכילים ביטוי בצורה סינתקטית בלבד

![Screenshot 2024-01-06 at 22.59.56.png](/img/user/Assets/Screenshot%202024-01-06%20at%2022.59.56.png)

__local definition__ זה דרך שבה מצמידים שם בתוך expression (למשל אם נרצה להשתמש בשם מסויים מביטוי א בתוך ביטוי ב).

למשל:

``` OCaml
# let d = 2 * 3 in d * 7;;
- : int = 42

# d;;
Error: Unbound value d
```

חשוב לשים לב שה name bound הגלובלי לפני ה `in` במקרה הזה $d$ מקבל את ההצמדה שלו רק אחרי ה `in`. כלומר , השם $d$ מקבל את הערך $6$ בתוך הביטוי $d*7$ .

השגיאה למעלה קוראת בגלל שאין global definition, ניסינו לגשת ל $d$ אבל אמרנו שבמקרה של הצהרה מקומית ההצמדה נעשה רק אחרי ה in ולא באופן גלובלי...
כמו כן, גם חשוב לשים לב שתמיד יבוצע הערכה למה שלפני ה  `in` במקרה הזה $2*3$ .

הסינתקס הוא `let ... = ... in`. 

ניתן לבצע שרשור להצהרות מקומיות
``` OCaml
# let d = 2 * 3 in
  let e = d * 7 in
  d * e;;
  
# d;;
Error: Unbound value d
# e;;
Error: Unbound value e
```

ניתן גם לעשות nesting 
```OCaml
# let d =
    let e = 2 * 3 in
    e * 5 in
  d * 7;;
- : int = 210

# d;;
Error: Unbound value d
# e;;
Error: Unbound value e
```

המשמעות של  `in` זה למעשה scope שבו לשם של המשתנה יש משמעות. כאשר לא רושמים `in` לשם יש משמעות באופן גלובלי (מאחוריי הקלעים utop מוסיף לזה in ומכניס לתוכו את כל הקוד שכתוב מתחת להשמה).
![Pasted image 20240106233915.png](/img/user/Assets/Pasted%20image%2020240106233915.png)

מאחורי הקלעים הרעיון הוא די פשוט: 
```Psuedo
    let x = 1 + 4 in x * 3
-->   (evaluate e1 to a value v1)
    let x = 5 in x * 3
-->   (substitute v1 for x in e2, yielding e2')
    5 * 3
-->   (evaluate e2' to v2)
    15
      (result of evaluation is v2)
```
בעצם מבצעים החלפה של `x` מימין ל `in` בערך שהתקבל עבורו משמאל... 
ההחלפה הזאת קורת בסמנטיקה הדינמית וצריך לשים לב איך מחליפים אם אנחנו נתקלים במצב של Overlapping scope למשל 
```OCaml
let x = 5 in
  ((let x = 6 in x) + x)

"will evaluate to"
let x = 5 in
  ((let x = 6 in 6) + 5)
```

נשים לב שבאמצעות הכלל הזה אנחנו יכולים להבין למה משתנים בOCaml הם immutable. 
למשל אם נרשום את הקוד 
```OCaml
let x = 1;;
let x = 2;;
```
זה שקול ל 
``` OCaml
let x = 1 in
	let x = 2 in 
		x
```

כפי שאמרנו ההצהרה החיצונית רלוונטית רק למה שבתוך הin אבל בעצם לא עשינו בו שום שימוש ולכן `x` מסתכם ב 2.
## If Expressions
הביטוי `if eq then e2 else e3`  מחשב את `e2` אם `e1 = true` ויחשב את `e3` אחרת.
בניגוד להצהרת `if else` בשפות אימפרטיביות, ב OCaml מתייחסים לביטוי הזה כמו כל ביטוי רגיל וניתן לשים אותם בכל מקום שניתן לשים כל ביטוי אחר (בדומה ל סינתקס המקוצר של תנאים בשפות אחרות `:?`). נשים לב שלמשל הביטוי הבא `4 + (if 'a' = 'b' then 1 else 2)` הוא תקים לגמרי ב OCaml. 

נשים לב שגם לביטויי if ניתן לבצע nesting: 

```OCaml
if e1 then e2
else if e3 then e4
else if e5 then e6
...
else en
```

ה else האחרון הוא חובה מסיבות של expression evaluation של השפה. אם נשמיט אותו סביר שנקבל שגיאה.

## Function
>[!warning] חשוב לשים לב
>מתודות ופונקציות הם לא אותו דבר. מתודה היא חלק מאובייקט שיכולה לשנות את הdata של אותו אובייקט באמצעות גישה ל `this`. פונקציות בשפות תכנות מונחות עצמים הם high order citizens כלומר הם משתנים לכל דבר והם גם הרמטיים, הם אינם משנים שום data חיצוני

הפונקציה הבסיסית ביותר בOCaml היא __פונקציה אנונימית__ ההצהרה עליה היא על ידי הסינתקס: `(fun x -> x+1)` אם נכתוב את זה ב utop נקבל 

```utop
- : int -> int = <fun>
```

הפלט `<>` מסמל משהו unprintable כלומר הוא לא יכול להדפיס בידיוק מה הפונקצייה עושה אלא הוא יודע שזה פונקצייה. הסינקס `int->int` מסמל את ערך הקלט של הפונקציה וערך החזרה של הפונקציה.

כדי להפעיל את הפונקציה פשוט שמים את הקלט ליד הפונקציה עצמה (לכן עטפנו פונקציה אנונימית בסוגריים) ` 3110 (fun x -> x+1)`  ללא הסוגריים היינו מקבלים שגיאת סינתקס.


>[!note] evaluation
>הסינתקס באופן כללי של פונקציה אנונימית הוא `fun x1 ... xn -> e`
>פונקצייה היא value - אין חישוב שנעשה בזמן ריצה כלומר $e$ לא מחושב עד שהפונקצייה נקראת בקוד. כמו כן המשמעות של זה היא שפונקציות ניתנות לשימוש בכל מקום שבו ניתן לכתוב ערכים. כמו כן, פונקציות יכולות לקבל פונקציות אחרות כפרמטרים ויכולות גם להחזיר פונקציות


>[!note] הבחנה 
>פונקציה אנונימית נקראת גם פונקצית למבדה ובכתיב מתמטי $\lambda x.e$ .

ניתן כמובן לתת לפונקציה גם שם למשל `val inc = fun x -> x + 1`.
וב syntactic sugar:
`let inc x = x+1` , פשוט שמים את הארגומנטים לפני ה $=$.

__חישוב של הפעלת פונקציה:__ 
הסינתקס להפעלת פונקציה הוא פשוט `e0 e1 e2 e3 ... en` בלי סוגריים אלא אם כן צריך סדר מסויים.

הפעולת חישוב של פונקציה נעשית באופן הבא:
	א)  המר כל $e_{i}\to v_{i}$ כאשר $v_{i}$ הוא ערך וגם $v_{0}$ הוא חייב להיות פונקציה `fun x1...xn->e` .
	ב) החלף $x_{i}$ ב $v_{i}$ מה שיניב ביטוי חדש $e^{\prime}$ שמורכב רק מערכים. 
	ג) חשב $e^{\prime}\to v$ .


### פונקציות רקורסיביות
באקומל חייבים להגדיר באופן מפורש על פונקציה רקורסיבית: `let rec f` .למשל נכתוב פונקצייה שמחשבת עצרת :
``` OCaml
(* requires: [ n >= 0]*)
let rec fact n =
	if n = 0 then 1
	else n * fact (n-1)
```

כיוון שבאופן כללי הניתוח של הביטוי הוא מימין לשמאל אם לא נגדיר את הפונקציה כרקורסיבית הקומפיילר לא ידע בזמן ריצה את המשמעות של fact ולא יוכל להפעיל אותה. 

הנה פונקצייה רקורסיבית נוספת:
``` OCaml
(** [pow x y] is [x] to the power of [y].
     Requires: [y >= 0]. *)
let rec pow x y = if y = 0 then 1 else x * pow x (y - 1)
```

ב utop נקבל פלט 
``` utop
val pow : int -> int -> int = <fun>
```

אם יש פונקציות רקורסיביות שמפעילות אחד את השני נוכל להשתמש ב `and` בסינתקס הבא-
``` OCaml
let rec f x1 ... xn = e1
and g y1 ... yn = e2
```
לדוגמה-
```OCaml
(** [even n] is whether [n] is even.
    Requires: [n >= 0]. *)
let rec even n =
  n = 0 || odd (n - 1)

(** [odd n] is whether [n] is odd.
    Requires: [n >= 0]. *)
and odd n =
  n <> 0 && even (n - 1);;
```
### Function types
פונקצייה יכולה להיות מטיפוס $t\to u$ כאשר $t$ הוא טיפוס הקלט ו $u$ הוא טיפוס הפלט.
פונקצייה יכולה להיות גם מטיפוס $t_{1}\to t_{2}\to u$ כאשר $t_{1},t_{2}$ הם טיפוסי קלטים.
באופן כללי פונקצייה היא מטיפוס $t_{1}\to t_{2}\to\dots t_{n}\to u$ . 

ל $\to$ יש משמעות כפולה שהוא מסמל גם על טיפוס הקלט וגם על טיפוס הפלט. 
בtype checking (הסמנטיקה הסטטית) שקובעת את טיפוס הפונקצייה פשוט מתבצעת בדיקה על טיפוס הקלטים (לאחר התהליך שנקרא type inference). באופן כללי פשוט בודקים 

```Psuedo
if assuming x1:t1, ..., xn:tn;
we can conclude:
e:u
Then
(fun x1...xn->e): t1->...->tn->u
```

בפונקציה רקורסיבית הבדיקה הייתה :
```Psuedo
if assuming x1:t1, ..., xn:tn; AND f : t1 -> t2 -> ... -> tn -> u
we can conclude:
e:u
Then
(fun x1...xn->e): t1->...->tn->u
```

באופן דומה גם הפעלת פונקציות מנותחת ב type checking.

``` Psuedo
if e0 : t1 -> ... -> tn -> u
AND 
	e1 : t1,
	e2 : t2,
	.
	.
	.
	en : tn 
THEN e0 e1 ... en : u
```

>[!info] הבחנה
>אין סמנטיקה דינמית בהגדרה של פונקציות שכן לא נעשה שום חישוב איתן עד שלא מפעילים אותן.
### Application operators
נסתכל על הפונקציה  `let succ x = x+1` . נשים לב שבסמנטיקה הדינמית OCaml הפענוח של פונקצייה הוא הדוק יותר מהפענוח של אופרטור כך שאם נרצה לחשב את `succ 2 * 10` המהדר בעצם מפרש את זה כ `10 * (succ 2)` .  יכול להיות שהתכוונו ל `succ (2 * 10)` . אופרטורים של הפעלת פונקציות הן מעין סינתקס סוכר למנוע מאיתנו לכתוב את הסוגריים למשל  
`succ @@ 2 * 10` נקרא __application operator__ והוא מפריד בין הקלט לפונקציה לקריאה שלה.
אופרטור נוסף הוא __reverse application operator__ או pipeline. 
נניח שנרצה להפעיל את הריבוע חזקה של `succ 5` כלומר $6^{2}$ . בסינתקס הרגיל נצטרך לרשום `square (succ 5)` . אם לא היינו עושים זאת , היינו מקבלים שגיאה כי המהדר היה מפרש את זה כ `5(square succ)` שזה לא קלט תקין לפונקציה שמקבלת int. נוכל להשתמש בreverse operator שדומה בעקרון שלו ל pipeline בתוכנות CLI. מעין דרך לשרשר value כקלט לפונקצייה הבאה אחריו ולאפשר קריאה רציפה משמאל לימין למשל במקרה שלנו 

```OCaml
5 |> succ |> square
```

ייקח את 5 ישרשר אותו כקלט ל succ ואת הפלט שיווצר ישרשר ל square. האופרטור `<|` נקרא אופרטור pipeline.

### Polymorphic Functions
נגדיר את פונקצייה הזהות `let id x = x` פונקציה שמחזירה את הקלט שאותו היא מקבלת. 
הטיפוס שנקבל עבור הפונקציה הזאת הוא `val id : 'a -> 'a` . המשמעות של `a'` הוא _type variable_ : והמשמעות של זה היא ״טיפוס לא ידוע/גנרי״. לשם הנוחות ארשום את המשתנים הללו באותיות לטיניות $`a=\alpha$  וכו..

הרעיון מאחורי היכולת הזאת היא [פולימורפיזם](https://he.wikipedia.org/wiki/%D7%A4%D7%95%D7%9C%D7%99%D7%9E%D7%95%D7%A8%D7%A4%D7%99%D7%96%D7%9D_(%D7%9E%D7%93%D7%A2%D7%99_%D7%94%D7%9E%D7%97%D7%A9%D7%91)) - כתיבת פונקציות שעובדות עבור מספר משתנים בלי קשר לטיפוסים שלהם. הסינתקס של פונקציות הזהות למשל מבטיח שבהינתן קלט מטיפוס כלשהו הפלט יהיה מאותו טיפוס בדיוק ולכן במצבים אלה פולימורפיזם יכול לעבוד.

זה מאוד מתאים לעבודה עם מבני נתונים שכן אין חשיבות ל data הנשמר במבנה הנתונים למשל ב[[Computer Science/Data Structures/Linear Data Structures#רשימה\|רשימה]] או במחסנית, פונקציות הפועלות על מבנה הנתונים לא אמורים להיות תלויות במידע הנשמר בתוכן.

נתבונן למשל בהצהרה הבאה
```OCaml
fun (f,g) -> (fun x -> f(g x)))
```
זהו משתנה גלובלי אנונימי, ננסה לנתח את הטיפוס שלו , קל לראות שזאת אכן פונקצייה פולימורפית שכן לא ניתן לפרש את הטיפוס.  אין ספק שהטיפוס הוא מסוג פונקציה, אך מה הקלט והפלט שלה?  לשם כך צריך להכיר את הקלט והפלט של $f,g$ .

ניתן לראות ש $f: (g(x)\to \beta)$ כלומר אנחנו לא יודעים מה טיפוס ההחזרה של $f$ אך אנחנו יודעים שהקלט שלו הוא הפלט של $g$ , הטיפוס של $g$ גם לא ידוע ולכן נגדיר $g(x): \alpha$ . כלומר $f:\alpha\to\beta$ . כעת נסתכל על $g$ , הקלט שלה הוא המשתנה $x$ שאנחנו לא יודעים מה הטיפוס שלו אז נסמנו $\gamma$ .
לכן $g:\gamma\to\alpha$ אז סך הכל הקלט לפונקציה האנונימית שלנו היא  $(\alpha\to\beta)*(\gamma\to\alpha)$.

מה לגבי הפלט?  אם כן הפלט הוא גם כן פונקציה שמקבלת את הטיפוס של $x$ הלוא הוא $\gamma$ והפלט הוא ערך ההחזרה של $f$ ולכן סך הכל הפונקציה היא מטיפוס 

$$(\alpha\to\beta)*(\gamma\to\alpha) \to (\gamma\to\beta)$$
### Partial Application 
נסתכל על הפונקציה הבאה  `add x y = x + y` פונקצית הוספה פשוטה. בOCaml יש פיצ׳ר שמאפשר לנו להפעיל פונקציות באופן חלקי כלומר אם נריץ `add 2 3` נקבל $5$ .

אבל אם נריץ `add 2` נקבל טיפוס מסוג `int -> int`. בעצם הפעלה באופן חלקי מחזירה פונקצייה שנוכל להפעיל עם שאר הפרמטרים כדי לקבל את הפונקציה המקורית. אם נריץ `3 (add 2)` נקבל $5$.

למעשה אפילו נוכל לשמור את הפונקציה הזאת, לדוגמה:`let add2 = add 2`.

הסיבה שזה עובר היא בגלל ש __באוקמל, פונקציות לא מקבלות באמת מספר פרמטרים__  אלא מה שעשינו עד כה היה מעין syntactic sugar. למשל:

```Psuedo
fun x y -> e

is syntactic sugar for

fun x -> (fun y -> e)
```

כלומר __פונקציות הן אסוציטיביות בOCaml__ בעצם 

```OCaml
let f x1 x2 ... xn = e

(*is semantically equivilant to*)

let f =
  fun x1 ->
    (fun x2 ->
       (...
          (fun xn -> e)...))

(*the type of such function*)
t1 -> t2 -> t3 -> t4
(*really means the same as*)
t1 -> (t2 -> (t3 -> t4))
```

ולכן כל פונקצייה מרובת משתנים היא בעצם שרשור של פונקציות שמקבלות פרמטר אחד כל פעם. העובדה שזה אפשרי היא בגלל ש __פונקציות הן ערכים__ אפשר להעביר פונקציות כפרמטר ולהחזיר פונקציות כפלט.

__curry__ 
זה קונספט של תכנות פונקציונלי שבו פונקציה שמקבלת מספר ארגומנטים היא מהווה בעצם רצף פעולות שכל אחד בה מקבל ארגומנט אחד.

let curry f x y = f (x, y)

![Pasted image 20240323134722.png](/img/user/Assets/Pasted%20image%2020240323134722.png)

## Loops
באוקמל אין באמת מימוש שדומה ללולאות בשפות אימפרטיביות (יש, אבל ב[[Computer Science/Programming Concepts/Programming Languages Principles/Imperative OCaml\|Imperative OCaml]]). הסיבה הכי קלאסית שאפשר לחשוב עליה שעבורה נרצה יכולת כזאת היא אלגוריתם [[Computer Science/Algorithms/Fibonacci algorithm\|פיבונאצ׳י]] שבמימוש הריקורסיבי שלו זמן הריצה שלו הוא מעריכי. אך באמצעות [[Computer Science/Algorithms/Dynamic Programming\|תכנות דינמי]] אפשר להגיע לזמן ריצה ליניארי. 

נתחיל מלהגדיר פונקציה `iter` משלנו ונסביר אותה.

```OCaml
let rec iter = fun n f x -> match n with 
	| 0 -> x 
	| n -> (f(iter (n-1) f x)
```

בעצם בנינו פה פונקציית איטרציה גנרית באמצעות ריקורסיה. הפונקציה מקבל את $n$ מספר האיטרציות, הפונקציה שיש לבצע בכל איטרציה $f$ ואת הערך הדיפולטי שמחזירים בסוף האיטרציה $x$.

כעת כדי לפתור את fib נוכל להגדיר 
```OCaml
let f = fun (x,y) -> (x+y,x);;
let second = fun (x,y) -> y;;
let fib = fun n -> second(iter n f (1,1));;
```
### While
נבנה פונקצייה שמממשת `while` : 

```OCaml
let rec _while = fun p f x -> if (p x) then x else (_while p f (f x))
```

כאשר $p$ הוא פרדיקט ו $f$ הוא גוף הלולאה. למשל נוכל ליצור לולאה אינסופית על ידי `p x = false` ו $f= id$ כלומר $f$ הוא פונקציית הזהות.  והפעלת הקוד 

```OCaml
_while p id x
```

>[!info] הבחנה
> אכן קיימים [לולאות](https://cs3110.github.io/textbook/chapters/mut/arrays.html) בOCaml אך הן לרוב באות יד ביד עם mutability שזה קונספט שאנסה להתרחק ממנו בסיכום זה כיוון שאנחנו מדברים על OCaml תחת הכובע של תכנות פונקציונלי שבו אין mutability בכלל
> 
## Data and Types
### Lists
רצף של ערכים מאותו טיפוס.
מאחורי הקלעים ממומשים בצורה של [[Computer Science/Data Structures/Linked list\|Linked list]]. רשימות בOCaml הן מאוד אינטואיטיביות , קלות להצהרה ולעבודה איתן, כך שאין צורך בעבודה מול ספרייה כדי להשתמש בהן, למשל כמו ב JAVA או C.

בנייה של רשימה היא באמצעות הסינתקס הבא 
```OCaml
[]
e1 :: e2
[e1; e2; ...; en]
```
כאשר מקבלים רשימה $lst$ ואלמנט $elt$  אפשר לשרשר את $elt$ מהסוף באמצעות הסינתקס `elt::lst` . הסוגריים המרובעות זה סינתקס נוח אך אינו חובה. כמו כן, כיוון שרשימות הן סוג של expression ניתן לבצע nestingשל רשימות עמוק ככל הניתן.

__סמנטיקה דינמית של רשימות__:
```OCaml
if e==> v1 and if  e2==>v2 then e1::e2 ==> v1::v2
```
כאשר ==> שקול ל evaluate to.
באופן כללי נוכל לרשום את הנ״ל כך: 

$$ \text{if  } \forall_{i\in \{1...n\}} \ :  (e_{i}==> v_{i})\  \text{ then} \ \ [e_{1};e_{2};\dots;e_{n}]==>[v_{1};v_{2};\dots;v_{n}]$$

__סמנטיקה סטטית של רשימות:__
א) `[]` הוא מטיפוס $\alpha \ \  \text{list}$  .
ב) שרשור של איבר עם רשימה מאותו איבר תייצר רשימה חדשה מאותו טיפוס.

__פונקציות חשובות ברשימות__
List.fold_left f acc lst - פונקציה שרצה על רשימה מפעילה על כל איבר את הפונקציה f ומאפשרת לצבור את המידע מכל הפעלה במשתנה acc. למשל אפשר לסכום את כל המערך על ידי let sum = List.fold_left (fun acc x -> acc + x) 0 lst. 
הטיפוס של הפונקציה val fold_right : ('a -> 'b -> 'b) -> 'a list -> 'b -> 'b.

List.fold_left f acc lst- אותו דבר רק מימין לשמאל.

List.map f lst - מחזירה רשימה חדשה שכל איבר בה הוא הפעלה של f על האיבר המתאים ב lst. 
let squared_lst = List.map (fun x -> x * x) lst

List.filter f lst - מחזירה רשימה חדשה של כל האיברים שעבורם הפרדיקט f מחזיר true. 
let even_numbers = List.filter (fun x -> x mod 2 = 0) lst

### Pattern matching
אחת הדרכים הנוחות ביותר לעבוד עם רשימות: Pattern matching. זאת הוא תכונה רבת עוצמה המאפשרת לנו לפרק ולהתאים מבני נתונים מורכבים בצורה תמציתית ואקספרסיבית. הוא משמש בדרך כלל עם סוגי נתונים כמו tuples, רשימות, variants וrecords. התאמת דפוסים משפרת את קריאות הקוד ומקלה על הטיפול במקרים שונים. 

ישנו דמיון רב בין Pattern matching ל switch case אך ההבדל הגדול הוא שכאן יש התאמה גם ב ״shape of data" ולא רק בערך עצמו, ובנוסף ניתן באמצעות pattern matching גם לחלץ חלק או חלקים מdata מסויים, מה שהופך את הכלי הזה לחזק הרבה יותר.

__סינתקס:__ באמצעות שימוש במילת המפתח `match` 

```OCaml
match expression with
| pattern1 -> result1
| pattern2 -> result2
| pattern3 -> result3
```

__patterns:__
כולל בתוכו - קבועים, משתנים, קונסטרקטורים, tuples , רשימות וכו׳.
קבועים ומשתנים תמיד מתאימים לעצמם וקונסטרקטורים מתאימים לערכים עם אותו קונסטרקטור. 
נסתכל על מספר דוגמאות 

```OCaml
(*matching constants*)
let check_number n =
  match n with
  | 0 -> "Zero"
  | 1 -> "One"
  | _ -> "Other"


(*matching tuples*)
let get_coordinates point =
  match point with
  | (x, y) -> "X: " ^ string_of_int x ^ ", Y: " ^ string_of_int y

(*matching lists*)
let rec sum_list lst =
  match lst with
  | [] -> 0
  | head :: tail -> head + sum_list tail


(*match records*)
type person = { name: string; age: int }

let get_name_age p =
  match p with
  | { name = n; age = a } -> "Name: " ^ n ^ ", Age: " ^ string_of_int a

(*matching variant*)
type shape = Circle | Rectangle of int * int | Square of int

let area s =
  match s with
  | Circle -> 3.14
  | Rectangle (width, height) -> width * height
  | Square side -> side * side
```

> [!info] wildcard
היכן שמסומן _ זה בעצם כמו default ב `switch` בשפות אימפרטיביות. זה נקרא wildcard
### Pattern matching with lists
רשימה יכולה להיות או ריקה, או שרשור של אלמנט עם תת רשימה. 
לכן נוכל לנצל את התכונה הזאת כדי לבנות סינתקס קבוע לסריקה של רשימה. למשל נבנה פונקצייה שבודקת אם הרשימה ריקה

```OCaml
let empty lst =
	match lst with
	| []->true
	| h::t -> false
```

באותו אופן נוכל לסכום את האיברים ברשימה 

```OCaml
let rec sum lst =
  match lst with
  | [] -> 0
  | h :: t -> h + sum t
```

>[!info] הבחנה
>אפשר לשרשר איבר ורשימה עם הסינתקס `::` או לשרשר שתי רשימות עם הסינתקס `@` .

רשימות אינן ניתנות לשינוי. אין דרך לשנות רכיב של רשימה מערך אחד למשנהו. במקום זאת, מתכנתי OCaml יוצרים רשימות חדשות מתוך רשימות ישנות. לדוגמה, נניח שרצינו לכתוב פונקציה שהחזירה את אותה רשימה כמו רשימת הקלט שלה, אבל עם האלמנט הראשון (אם יש כזה) מוגדל באחד. יכולנו לעשות את זה: 
```OCaml
let inc_first lst =
  match lst with
  | [] -> []
  | h :: t -> h + 1 :: t
```

אין צורך לחשוש מזכרון מקום, כיוון ש `t` משותפת בין הרשימה הישנה לחדשה כך שכמות הזכרון היחידה שמתווספת היא הזכרון הדרוש כדי לשרשר את `h+1` לרשימה. 
הקומפיילר עושה את זה בגלל שכל דבר בתכנות פונקציונלי הוא בלתי ניתן לשינוי וכך גם האיברים ברשימה ולכן אין חשש מכך ששיתוף זכרון ייצור שינוי באמינות המידע. שכן כל שינוי של משתנה מייצר עותק שלו.

>[!warning]
>בסמנטיקה הstatic, כאשר מדובר ב pattern matching הקומפיילר יעיר לנו אם לא כיסינו את כל הpatterns הדרושים בקוד שלנו. כמו כן , נקבל גם הערה אם יש לנו כפילויות בpatterns.
### Variants
זה טיפוס נתונים שמייצג ערך אחד מתוך מספר אפשרויות. בבסיס שלהם , השימוש מאוד דומה ל enum ב C. 

```OCaml
type day = Sun | Mon | Tue | Wed | Thu | Fri | Sat
let d = Tue
```

כל אחד מהערכים שהtype מקבל נקרא constructors. אם נרצה לגשת לconstructor המתאים עבור ערך כלשהו המימוש הוא פשוט באמצעות pattern matching

```OCaml
let int_of_day d =
  match d with
  | Sun -> 1
  | Mon -> 2
  | Tue -> 3
  | Wed -> 4
  | Thu -> 5
  | Fri -> 6
  | Sat -> 7
```

>[!info] Scope
>במצב שבו יש שני קונסטרקטורים עם אותו שם עבור שתי וריאנטים הטיפוס שהוצהר אחרון מבנהם מנצח. 

לכל קונסטרקטור אפשר להצמיד ערכים באמצעות מילת המפתח `of`. 

```OCaml
 type commit =
  | Hash of string
  | Tag of string
  | Branch of string
  | Head
  | Fetch_head
  | Orig_head
  | Merge_head;;
type commit =
    Hash of string
  | Tag of string
  | Branch of string
  | Head
  | Fetch_head
  | Orig_head
  | Merge_head

```

אפשר להעביר מספר פרמטרים באמצעות הסינטקס `p1*p2` ואפשר להצהיר עליהם כ tuple באמצעות סוגריים `(p1*p2)`.

הכוח האמיתי של variant הוא באמצעות __טיפוסים רקורסיביים__ למשל [[Computer Science/Data Structures/Binary-trees\|עצים]] 

```OCaml
type 'a tree =
| Leaf
| Node of 'a * 'a tree * 'a tree


(* the code below constructs this tree:
         4
       /   \
      2     5
     / \   / \
    1   3 6   7
*)
let t =
  Node(4,
    Node(2,
      Node(1, Leaf, Leaf),
      Node(3, Leaf, Leaf)
    ),
    Node(5,
      Node(6, Leaf, Leaf),
      Node(7, Leaf, Leaf)
    )
  )
  
```

כעת אפשר לבצע פעולות על המבנה הרקורסיבי שהגדרנו 

```OCaml
let rec preorder = function
  | Leaf -> []
  | Node {value; left; right} -> [value] @ preorder left @ preorder right
```

>[!info] הבחנה
>נשים לב שבנינו את העץ שלנו גם רקורסיבי וגם פולימורפי. זה אפשרי כפי שאפשר לכתוב פונקציות פולימורפיות

>[!info] הבחנה
>רשימות הן גם variant שמוגדר באופן הבא:
> `type a' list = Nil | Cons of a' * (a' list)` 


>
