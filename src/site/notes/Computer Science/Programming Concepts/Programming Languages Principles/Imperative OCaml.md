---
{"dateCreated":"2024-03-21 20:21","tags":["programming_language"],"pageDirection":"rtl","dg-publish":true,"permalink":"/computer-science/programming-concepts/programming-languages-principles/imperative-o-caml/","dgPassFrontmatter":true}
---

# Exceptions
ב[[Computer Science/Programming Concepts/Programming Languages Principles/OCaml\|OCaml]], שגיאות נעשות באמצעות exceptions שמוגדרים על ידי הטיפוס exn. הטיפוס שלו הוא variant מהצורה

```OCaml
type exn =
	| Division by zero
	| Failure of String 
```

ניתן להוסיף שגיאות בצורה דינמית באמצעות המילה השמורה exception 

```OCaml
exception Int_exception of int;;
```

ניתן לזרוק exceptions באמצעות המילה השמורה raise

```OCaml
let hd = fun l -> match l with 
	| [] -> raise(Int_exception 2)
	| h::t -> h;;
```

תפיסת שגיאות עם try with 

```OCaml
try 1/0 with
Int_exception i -> i
| Failure s -> 5
| Division_by_zero -> 0;;
```

# פונקציות ללא ערך החזרה
אין פונקציה שלא מחזירה כלום ב OCaml יש טיפוס מסוג unit שהערך היחיד שלו הוא ( ). למשל 

```OCaml
print_string("a\n");;
a
-: unit = ()
```

אם יש $e_{1}$ ביטוי מטיפוס unit ו $e_{2}$ ביטוי מטיפוס T אז $e_{1};e_{2}$ הוא ביטוי מטיפוס T שערכו זהה ל $e_{2}$.

```OCaml
print_string("a\n");5;;
a

-: int = 5;
```

# mutability 
בדיפולט, משתנים הם immutable ב OCaml כלומר לא ניתן לשנות את ערכם מבלי לייצר עותק חדש. 

ישנם מספר מבני נתונים אימפרטיבים שכן תומכים בשינוי state.

## vectors
הסינטקס שלהם הוא כמו רשימות אבל הגודל שלהם קבוע. כלומר זה מערך חד מימדי סטטי.

```OCaml
(* Define a vector of integers *)
let vector = [|1; 2; 3; 4; 5|]

(* Define a vector of integers *)
let vector = [|1; 2; 3; 4; 5|]

(* Accessing elements of the vector *)
let first_element = vector.(0)   (* Access the first element (indexing starts from 0) *)
let third_element = vector.(2)   (* Access the third element *)

(* Modifying elements of the vector *)
vector.(1) <- 10  

(* Printing the modified vector *)
Array.iter (fun x -> print_int x; print_string " ") vector;
print_newline()
```

נרצה לכתוב את פונקציה שמקבלת וקטור ושני אינדקסים ומחליפה בין האינדקסים בוקטור המקורי

```OCaml
let swap v i j = let x  = v.(i) in 
	(v(i) <- v.(j) ; v.(j) <- x);;
```

השורה let x = v.(i) מאפשרת לנו להחזיק משתנה זמני. 
## mutable structures
נסתכל על מבנה הנתונים הבא
type point = {x: float ; y: float}. 

כעת נגדיר פונקציה שמוסיפה נתונים לנקודה 
```OCaml
let add = fun ((dx, dy), p) -> {x = p.x +. dx; y =p.y +. dy}
```
ניתן לראות שהפונקציה שלנו מחזירה נקודה חדשה ולא מעדכנת את הנקודה שהבאנו כקלט.

אם נרצה להפוך אותו ל mutable נוכל לעשות את זה בקלות באופן הבא

```OCaml
type mpoint = {mutable x : float ; mutable y : float}
```

כעת נבנה פונקציה חדשה שדומה לadd אבל עובדת עם mutable point 

```OCaml
let move = fun ((dx , dy), p) -> p.x <- p.x+dx ; p.y <- p.y + dy;;
```

נשים לב שאם נריץ את move נקבל שערך החזרה הוא unit בניגוד להרצה של add ששם ערך החזרה יהיה point .

>[!note] לולאות באוקמל אימפרטיבי
>בתכנות אימפרטיבי באוקמל כן יש תמיכה בלולאות 
>![Screenshot 2024-03-23 at 13.35.21.png](/img/user/Assets/Screenshot%202024-03-23%20at%2013.35.21.png)
# Compare by value and reference 
בOCaml יש 
* = שיוויון ערכים/מבני
* == שיוויון זהות/פיזי (ממש אותו כתובת בזכרון)
* האופרטור <> הוא השלילה של שוויון ערכים.
* האופרטור =! הוא השלילה של שיוויון זהות.

```OCaml
let mp1 = {x=1.0, y=1.0};;
let mp2 = {x=1.0, y=1.0};;

mp1=mp2 
mp1==mp2

let mp3 = mp1;
mp3==mp1;
```

השיוויון הראשון יחזיר true והשני יחזיר false. לבסוף השיוויון השלישי יחזיר true בגלל שאלו ממש אותם אובייקטים.

__references__
ref הוא מילה שמורה שמגדירה reference. בדומה לשפת c , ניתן לעדכן את הערך של הref, ניתן לחלץ את הערך מהref וניתן לעדכן את הערך ששמור בתוך ה ref.

```OCaml
let c = ref 0;;
(** val c : int ref = {contents = 0};**)

 c:= 5;; (** update the value in the reference **)

!c;; (*access the value of the reference*)
```

נוכל לפתח את זה לקונספטים יותר מעניינים למשל רשימה של ref 

```OCaml
let l = [ref 1; ref 2; ref 3];;
(** var l: int ref list = [{...}]**)

(*lets try to extract the first element from the list*)
List.hd(l)
(** int ref = {contents = 1} **)

!List.hd(l);;
(** Error- OCaml will first try to search the memory address if the inerpreter sees '!', before evaluate the function **)

!(List.hd(l));;
(*** int = 1 *)

List.hd(l) := 10;;
(** will update the first element in the list to point to value 10 **)
```

אחד השימושים הנפוצים של רפרנסים באוקמל הוא בניית פונקציות שיש להן internal state. למשל נוכל להחזיק פונקציה שיש לה ref לאיזה פרמטר ובגלל שזה ref הוא לא ייעלם בין הקריאות לפונקציות.

```OCaml
let gensym = let counter = ref 0 in fun() -> counter := !counter + 1; "sym"^string_of_int(!counter);;

(** val gensym : unit -> string = <fun>**)
```

הפונקציה מחזירה את מה שבצד ימין (אחרי ה ; ) שזה שרשור של sym עם counter שעולה ב 1 בכל פעם שקוראים לפונקציה.

_נשים לב שהאופרטור =: מייצג reference assignment והאופרטור ! מייצג הוצאת הערך מתוך ה ref_.

__שיוויון של מצביע__
* כאשר בודקים בודקים שיוויון מבני בין מצביעים משווים את התוכן שלהם.
* כאשר בודקים שיוויון פיזי בין מצביעים משווים את הכתובת שלהם.

יכולה להיות בעיה בשיוויון מבני כאשר עובדים עם מבני נתונים שמאפשרים כפילויות בין אובייקטים שונים או במבני נתונים שמאפשרים מעגליות למשל ברשימה מקושרת מעגלית:

```OCaml
type 'a rlist = 
	| Nil
	| Cons of 'a * ('a rlist ref);;

let newcell x y = Cons(x, ref y);;
let updnext (Cons (_,r)) y = r := y;;
```
![Screenshot 2024-03-23 at 13.27.29.png](/img/user/Assets/Screenshot%202024-03-23%20at%2013.27.29.png)

אם נשווה את מבנה הנתונים הזה לפי מבנה נקבל לולאה אינסופית שכן OCaml ינסה לעבור על המבנה עד שהוא מגיע ל null ולהשוות תא תא, אבל אם תהיה רשימה מקושרת מעגלית הוא יתקע. רק אם נשווה בין כתובות נקבל true. 

![Screenshot 2024-03-23 at 13.32.57.png|200](/img/user/Assets/Screenshot%202024-03-23%20at%2013.32.57.png)

# Modules 
כדי להגדיר מודולים משתמשים במילים השמורות הבאות 
1) module
2) type
3) sig
4) val 

מגדירים ראשית module type שזה דומה לinterfaces ברעיון. 

```OCaml
module type LIST_STACK = sig
  exception Empty
  val empty : 'a list
  val is_empty : 'a list -> bool
  val push : 'a -> 'a list -> 'a list
  val peek : 'a list -> 'a
  val pop : 'a list -> 'a list
end
```

הפיצ׳ר הזה נקרא signatures. 
* נשים לב שכאן מגדירים val ולא let 
* השם של המודול צריך להתחיל ב אות גדולה. 
* חייב לסיים ב end

באופן הזה אנחנו יכולים להפריד בין המימוש לבין ה״ממשק״ שמי שמשתמש במודול יכול לראות 

```OCaml

module type LIST_STACK = sig
  (** [Empty] is raised when an operation cannot be applied
      to an empty stack. *)
  exception Empty

  (** [empty] is the empty stack. *)
  val empty : 'a list

  (** [is_empty s] is whether [s] is empty. *)
  val is_empty : 'a list -> bool

  (** [push x s] pushes [x] onto the top of [s]. *)
  val push : 'a -> 'a list -> 'a list

  (** [peek s] is the top element of [s].
      Raises [Empty] if [s] is empty. *)
  val peek : 'a list -> 'a

  (** [pop s] is all but the top element of [s].
      Raises [Empty] if [s] is empty. *)
  val pop : 'a list -> 'a list
end

```

* הוספנו הערות לmodule type, שכן מי שישתמש במודול הזה יוכל לראות רק את הממשק ולא את המימוש.
* יכולים להיות מספר מימושים לאותם module types , הפונקציות הפנימיות של אותם מימושים לא יהיו חשופים למי שישתמש במודול.

כעת נוכל לממש את המודול עם struct.

```OCaml
module ListStack : LIST_STACK = struct
  let empty = []

  let is_empty = function [] -> true | _ -> false

  let push x s = x :: s

  exception Empty

  let peek = function
    | [] -> raise Empty
    | x :: _ -> x

  let pop = function
    | [] -> raise Empty
    | _ :: s -> s
end
```

נשים לב שהמודול צמוד ל module type שהגדנו. לכן כל מימוש נוסף שנמצא בתוך ListStack לא יחשף מחוץ לmodule עצמו.

* ניתן להגדיר מודולים בלי להצמיד להם type
	* במצב זה OCaml יגדיר את זה בעצמו כפי שהוא עושה גם לפונקציות.

* אם הצמדנו בין מודול ל module type חובה לממש את כל הפונקציות בtype.

>[!note] 
>מודול אינו מחלקה (אין this) אלא רק אוסף של הגדרות.

__הרחבת מודול__
ניתן לקחת מודול קיים ולהוסיף לו פונקציות כרצוננו

![Screenshot 2024-03-23 at 13.14.39.png|250](/img/user/Assets/Screenshot%202024-03-23%20at%2013.14.39.png)

עם הסינתקס include List יצרנו מעצם הרחב למודול של List. זה נקרא extensions, זה קונספט שיש אותו במספר שפות תכנות. במקרה הזה הפעולה לייצר מודול חדש שנקרא Extension.List ותוסיף את הפונקציה הזאת לשם. 