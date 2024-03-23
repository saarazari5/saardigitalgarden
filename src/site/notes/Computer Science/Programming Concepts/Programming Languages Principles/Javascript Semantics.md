---
{"dateCreated":"2024-03-16 01:08","tags":["programming_language"],"pageDirection":"rtl","dg-publish":true,"permalink":"/computer-science/programming-concepts/programming-languages-principles/javascript-semantics/","dgPassFrontmatter":true}
---

# javascript
לרוב [[Computer Science/Programming Concepts/HTML - CSS - Javascript#JavaScript\|javascript]] היא שפה המיועדת ל[[Computer Science/Programming Concepts/HTML - CSS - Javascript\|web]] 
אבל יש לשפה כמה תכונות מעניינות שנרצה לעסוק בהן. 

ראשית, השפה היא duck type checking כלומר שהיא כמעט לא עושה בדיקת טיפוסים, היא רק בודקת האם כל הפונקציות שאובייקט מסויים מפעיל אכן מוגדרות אצל האובייקט הזה. נזכיר שפרדיגמות של type checking נוספות הן static type כמו ב [[Computer Science/Programming Concepts/Programming Languages Principles/OCaml\|OCaml]] שנעשות בזמן קומפילציה או dynamic type שבה ה [[Computer Science/Programming Concepts/Programming Languages Principles/Type Inference\|Type Inference]] נעשה בזמן ריצה.

הטיפוסים והערכים של הפונקציה הם
1) מספרים 
2) מחרוזות
3) בוליאניים
4) מערכים
5) null
6) undefined
7) __משתנים__
8) __פונקציות__ 
9) __אובייקטים__

## משתנים
* הצהרה על משתנים כדאי עם המילה var. 
	* לא חובה לבצע עליו השמה, במידה ולא עושים מתקבל ערך מידי של undefined.
	* ניתן לעדכן ערכים של משתנים.
	* בידה ומצהירים על משתנה ללא המילה var הוא מקבל scope גלובלי.
* scope 
	* משתנה גלובלי הוא מוגדר בכל מקום בקוד
	* משתנה לוקאלי הוא משתנה שמוגדר רק בפונקציה מסוימת
	* shadowing
	* hoisitng

נסתכל על הקוד הבא כדי להבין מה קורה בshadowing וב hoisting

```js
var scope = "global"

function f() {
	console.log(scope)
	var scope = "local"
	console.log(scope)
}
```

היינו מצפים שהפעלת f שהפלט יהיה `global` ולאחר מכן `scope` אבל זה לא המצב בגלל שבjavascript האינטרפטר מבצע הפרדה בין הצהרה על משתנה להשמה שלו באופן הבא

```javascript
var scope = "global"
function f() {
	var scope;
	console.log(scope);
	scope = "local";
	consol.log(scope)
}
```

זה גורם לפלט של f להיות undefined ולאחר מכן local. מצב זה שבו יש שני שמות זהים בשני scopes שונים נקרא shadowing והפעולה של העלאת ההצהרה של המשתנה לתחילת ה scope נקראת hoisting.

אם היינו מורידים את שלב ה declaration כלומר בלי var בתוך f היינו מקבלים את הפלט הרצוי.

נשים לב ש undefined זה לא אותו דבר כמו המצב של משתנה שמוגדר לאחר השימוש. נסתכל למשל על הפונקציה הבאה

```javascript
function f() {
consol.log(scope)
scope = "local"
console.log(scope)
}
```

בקטע הקוד הזה נקבל שגיאה בגלל שלא השתמשנו ב var. ברגע שהinterpeter מזהה את מילת המפתח var הוא מבצע hoisting למשתנה שזה העלת ההצהרה שלו לתחילת ה scope שלו. 
ברגע שאין var הוא לא עושה זאת ולכן בשורת ההדפסה הראשונה אין משתנה שמוגדר כך גם לא מבחינת ההצהרה ולכן תזרק שגיאה.

כדי לתקן את זה, רק צריך לשים var בשורה שבה ביצענו השמה ל scope. 

![Pasted image 20240316015317.png](/img/user/Assets/Pasted%20image%2020240316015317.png)

נראה דוגמה נוספת על ידי הסתכלות על שני קטעי הקוד הבאים, ההבדל בינהם הוא שבאחד יש var ובשני אין עבור ההצהרה על j .

בקוד השמאלי נקבל 0 ואז 0 בגלל ש j הוא ב global scope ובקוד השני תזרק שגיאה בגלל שהkeyword var מגדיר אותו להיות ב function scope של f.

 _התוכן של משתנים יכול להיות מהצורה ההבאה:_
 1) ערכים בסיסיים - מספרים, בוליאנים, null ו undefined. 
 2) אובייקטים - נשמרים by reference.
 3) מערכים - נשמרים by reference.
 4) פונקציות- נשמרים by reference

 >[!note] האובייקט הגלובלי
 >משתנים בscope גלובלי הם שדות של האובייקט הגלובלי. ניתן לגשת אליו באמצעות this בכל scope שהוא לא class scope.

>[!info] namespaces 
>ביחס בין משתנים לפונקציות נשים לב שאם יש לי משתנה לא מוצהר a ואחריו פונקציה עם אותו שם a אז כשנבדוק מה הטיפוס של a נקבל שהוא פונקציה אבל אם יהיה משתנה מוצהר a=n למשל. אז הטיפוס יהיה הטיפוס של n
>![Pasted image 20240323023013.png](/img/user/Assets/Pasted%20image%2020240323023013.png)

__שיוויון בין משתנים__
ניתן לבצע שתי פעולות השוואה ב js
1) זהות - ===
2) שיווין - ==

ההבדל בינהם הוא הוא שבהשוואה מסוג שוויון מתבצע type coercion כלומר המרה של ערך מטיפוס אחד לאחר.

לדוגמה אם 

```javascript
1==='1' //false
1=='1' //true
```
## custom closure
נסתכל על הקוד הבא

```JavaScript
function makeCounter() {
	var counter = 0;
	function make() {
		return ++counter;
	}
	return make;
}
```

אמרנו שאחד היכולות של [[Computer Science/Programming Concepts/Programming Languages Principles/Closures\|Closures]] הוא היכולת להסתיר מידע של הפונקציה. כאן למשל עבור כל קריאה לmakeCounter יש לנו בתוך הscope של הפונקציה את המשתנה counter שהוא נגיש רק בתוך ה scope הזה. כל קריאה ל makeCounter תייצר scope חדש עם counter חדש אבל כל עוד נמשיך לקרוא לאותו make הcounter יגדל (ככה ניתן למדוד את מספר הקריאות לפונקציה למשל)

דוגמה נוספת:
![Pasted image 20240323023717.png|250](/img/user/Assets/Pasted%20image%2020240323023717.png)

המשתנה i מקושר פעם אחת לפונקציה ולכן כל הערכים שמתקבלים הם 10. בגלל שvar עובר hoisting לתחילת הפונקציה אז i הוא בסקופ של constfuncs ולכן ערך החזרה מכל הפונקציות במערך יהיה 10. קל להתבלבל ולחשוב שמדובר ב dynamic scope אבל עדיין מדובר ב static scope כי מה שקורה זה שפשוט הסביבה שבה הפונקציה הוגדרה היא הסביבה של constfuncs שבה i=10 בסיום הריצה של הפונקציה. אם js הייתה עובדת בשיטת dynamic scope היינו מקבלים שגיאה כי בסביבה שבה הפונקציה נקראת אין משתנה i בכלל.

__איך נתקן?__
נוכל ליצור closure חדש לכל משתנה i. 

![Pasted image 20240323025035.png](/img/user/Assets/Pasted%20image%2020240323025035.png)

בעצם יצרנו פה scope חדש עבור i כאשר העברנו אותו כקלט ל g. בגלל שהעברנו את i כקלט לפונקציה חדשה by value אז עכשיו נשמר עבור כל $funcs[i]$ הערך i המתאים. 

הפתרון השני הוא להחליף את var ל let , כי let הוא סגור גם ל loop scope ול if scopes כשעושים hoisting.

## פונקציות
הצהרה: 
1) מילה שמורה function
2) בנאי Function + המילה השמורה new
3)  literal - פונקציות אנונימיות

```js
// 1)
function f(x) {x*x}

// 2) way slower method 
var f = new Function("x", "return x*x")

// 3)
var f = function(x){return x*x}
//also can be done like this:
//good for recursion
var f = function g(x) {return x*x}

// triggering g will cause ReferenceError from any scope outside g

```

__שימוש בפונקציות__
1) אם אין return אז יוחזר undefined
2) לא מציינים טיפוסים
3) call by value
4) אפשר להגדיר פונקציות בתוך פונקציות אחרות למשל

```js
var foo = function() {
	var a = 3
	var b = 5
	var bar = function () {
		var b = 7
		var c = 11
		a= a+b+c
	}
	console.log(a,b)
	bar()
	console.log(a,b)
}

/**
3 5 
21 5
/**
```

5) פונקציות יכולות להיות שדות של אובייקטים ובכך להפוך ל ״מתודות״
```js
var o = { f: function() {return 5}}

o.f()
```

>[!note] 
>ניתן להשתמש בפונקציות אנונימיות כדי ליצור custom scope למשל 
>function (){var x =4;x} ייצור scope ספציפי למשתנה x שמחוץ לו הוא לא נגיש. כמו כן ניתן להפעיל פונקציות אנונימיות ישירות על ידי עטיפתן בסוגרים והפעלתן למשל 
>()(function (){var x =4;x})

>[!note] הבחנה
>בגלל שפונקציות הן גם משתנים אז גם עליהן js עושה hoisting. למשל בקטע הקוד למטה מודפס 8 כי הערך התחתון דורס את העליון בגלל hoisting ו shadowing 
>![Pasted image 20240323025621.png|200](/img/user/Assets/Pasted%20image%2020240323025621.png)
### ארגומנטים
פונקציה היא אובייקט, לקריאה שלה יש שדה args. 
js לא בודקת תקינות של מספר הארגומנטים.  

נסתכל על הפונקציה הבאה

```JS
function f() {
	if(arguments.length <= 1){
		return 0
	}else {
		var result = 1
		for(var i = 0; i< arguments.length; i++) {
			result = result * arguments[i];
		}
	return result
	}
}
```

ניתן לראות כאן שאנחנו משתמשים בשדה arguments שיש לכל פונקציה, למרות שהיא הוגדרה ללא חתימה עדיים אם נעביר מספר ארגומנטים הם יישמרו בפרמטר הזה.

עבור f בלי פרמטרים נקבל 0. אם נקרא למשל ל f(1,2) נקבל 2 כי מחשבים את מכפלת כל הארגומנטים.

arguments הוא שדה בתוך מופע של האובייקט של הפונקציה כלומר מופע שמייצג ריצה כלשהי של הפונקציה. 

נשים לב ש arguments הוא אובייקט בפני עצמו והוא מכיל עוד שדות. שדה מעניין נוסף שיש לו נקרא __callee__ שהוא מחזיר את הפונקציה עצמה וגם אותה ניתן להפעיל. למשל 

```js
function(x){if (x<=1) return 1; else return x * arguments.callee(x-1)}
```

callee מחזיק בתוכו גם פרמטר length אלא שהוא מייצג את מספר הפרמטרים שהפונקציה מקבלת כקלט בחתימה שלה. 

```javascript
function check (x,y) {
	var actual = arguments.length
	var expected = arguments.callee.length

	console.log(actual)
	console.log(expected)
}

check() 
// will print  0 2
```


__this keyword__
מה קורה כששמים this בתוך פונקציה. נסתכל על הפונקציה הבאה

```js
function f(x) {
	return this.a + x;
}
```

הפעלה של f בצורה רגילה תגרום ל this להתייחס לאובייקט הגלובלי כפי שכבר אמרנו ותזרק שגיאה כי אין לאובייקט זה משתנה מסוג a. 

__אבל__ ב javascript בניגוד לשפות אחרות מונחות עצמים כמו java, אני יכול להצמיד לפונקציה את המופע הרלוונטי (בד״כ לכל אובייקט יש מתודות).

את זה ניתן לעשות על ידי שני פונקציות שיש לאובייקט function 
1) call 
2) apply

שני הפונקציות הנ״ל מקבלות כקלט את האובייקט שיהיה this בתוך הפונקציה ואת הקלט לפונקציה עצמה למשל 

```js
o1 = {a:1};
o2 = {a:2};

f.call(o1,5)
f.call(o2,5)

f.apply(o2, [5])
```
כעת this לא יקרוס בגלל שלשני האובייקטים יש פרמטר a. ההבדל בין apply ל call זה שב apply הקלט לפונקציה מתקבל בתוך מערך.

>[!note] this במתודה
>כפי שאמרנו, this בתוך מתודה מתייחס למופע של האובייקט שבתוכו מוגדרת המתודה.

## אובייקטים
מכפלות עם שמות (variance) אבל שדות יכולים להיות עם פונקציות.

כדי לייצר אובייקט ניתן להשתמש בשני דרכים
1)  new+constructor 
2) ליטרלים

```js
//1)
var a = new Object()
var now = new Date(2024,2,25)

//2)
var circle = {x:0, y:0, radius:2}
```

__סמנטיקה:__
1) שדות - ניתן להוסיף גם שדות בצורה דינמית. 
```js
var book = new Object()
book.title = "catch 22"
book.numOfChapter = 3

// printing an object will print it JSON format
console.log(book)
```
2) אנומרצייה של שדות
```js
// iterate every variable name in object
for (var name in book) {
	//access the value:
	book[name]
}
```
1) אם שדה לא הוגדר הוא undefined
2) אפשר למחוק שדות
```js
//remove the variable title from book
delete book.title
```

__constructor__
* בנאי הוא פונקציה שקוראים לה עם המילה השמורה new. 
* הפונקציה מקבל באופן סמוי הפנייה לאוביקט שבונים ולרוב היא לא מכילה return בתוכה.
* לבנאי יש גישה לthis שמייצג את האובייקט שעומד להיווצר.
* משמש להגדרת מחלקות (הצהרה על מחלקה = הצהרה על בנאי)

```js
function Rectancle(w,h) {
	this.width = w;
	this.height = h;
}

var rect = new Rectangle(1,2)

consolt.log(rect)

// Rectangle {width: 1 , height: 2}
```

## prototype
1) לכל אובייקט יש prototype
2) לכל פונקציה יש שדה prototype למקרה וישתמשו בה כבנאי.
3) שדות ומתודות משותפות - לא משוכפלות

>[!note] מהדוקומנטציה
In JavaScript, every object has a prototype. The prototype is an internal reference that points to another object. When you try to access a property or method on an object that doesn't exist on the object itself, JavaScript will look for it in the object's prototype, and if it's not found there, it will continue searching up the prototype chain until it reaches the top-level Object.prototype.

נסתכל על הקוד הבא לדוגמה

![Pasted image 20240316112407.png|300](/img/user/Assets/Pasted%20image%2020240316112407.png)

הוספנו שתי פונקציות חדשות לprototype והן התווספו לכל המופעים של circle.
זה מאפשר לנו לממש ירושה:
1) כל אובייקט יורש את כל השדות של הprototype שלו
2) הפרוטוטייפ תמיד יורש מ Object.prototype
3) בגלל שגם prototype הוא אובייקט אז הוא מכיל prototype משלו, מה שנקרא prototype chain וזה נגמר רק ב Object.prototype. כאשר אנחנו מנסים לגשת לפונקציה מסויימת שלא נמצאת באובייקט javascript יעלה בהירכיית ה prototype וינסה לחפש את המימוש , אם לא ימצא יזרוק שגיאה.

כדי להגדיר ירושה אני צריך להגדיר שהprototype שלי הוא אובייקט מסוג מסויים למשל אם נרצה שאובייקט מסוג Complex יהיה אבא של אובייקט מסוג AdvancedComplex נצטרך להגדיר 

$$\text{ AdvancedComplex.prototype = Complex}$$

ככה Advanced יקבל את כל התכונות של Complex.
לכל האובייקטים יש את השדות הבאות

```
__defineGetter__
__defineSetter__
__lookupGetter__
__lookupSetter__
__proto__
constructor
hasOwnProperty
isPrototypeOf
propertyIsEnumerable
toLocaleString
toString
valueOf
```

אלא כלים ש javascript משתמש בהם כדי לממש inheritance.

