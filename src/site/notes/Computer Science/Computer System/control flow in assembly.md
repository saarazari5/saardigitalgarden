---
{"dateCreated":"2022-12-23 22:09","tags":["computer_system","assembly"],"pageDirection":"rtl","dg-publish":true,"permalink":"/computer-science/computer-system/control-flow-in-assembly/","dgPassFrontmatter":true}
---


# control flow in assembly

## Conditions
כדי להתחיל לדבר על תנאים נרצה לדבר על register מיוחד, כבר ציינו ב [[Computer Science/Computer System/Program structure in assembly\|מבנה התוכנית באסמבלי]] שיש שתי רגיסטרים מיוחדים שקשורים למחסנית, כעת נדבר על ה [flags register](https://en.wikipedia.org/wiki/FLAGS_register), הregister הזה מחזיק בתוכו מספר ביטים חשובים, (נתמקד בארבעת הבאים)
*   ZF-zero flag
*   SF- sign flag
*   CF- carry flag
*   OF-overflow flag
אלו מאפשרים לנו לקבל אינדיקצייה על תוצאות של חישובים אריתמטיים (שנעשים על בסיס הפעולות האריתמטיות ולא עם פקודת lea שמטרתה לבצע חישובים על כתובות).

### zero flag
אם התוצאה של פעולה אריתמטית היא 0, אז הflag הזה יידלק כלומר ערכו יהיה 1. למשל

$$Addb\ 0,0\rightarrow ZF=1$$

### sign flag
מקבל והתוצאה של מספר אם התוצאה היא שלילית כלומר MSB הוא 1.

$$subb \ 00000000_{b}, 00000001_{b}\rightarrow 11111111_{b}\rightarrow SF=1$$

### carry flag
משתמשים בו עבור חישובים של unsigned.
ערכו יהיה אחד אם התוצאה גדולה יותר מהקיבולת של אופרנד. למשל

$$11111111_{b}+ 00000001_{b}= 1\underbrace{00000000}_{8\cdot 0}=0\rightarrow CF=1$$

נשים לב שרק לפי הפקודות שנשתמש בהם הקומפיילר יודע האם העבודה היא עם signed או unsigned .

נראה פעולה שעבורה ה carry flag היה כבוי אם היינו עובדה באריתמטיקה של signed

$$00000000-00000001= 11111111$$

התוצאה אם אנחנו עם unsigned יוצאת 255 שזה כמובן שגוי ולכן ה cf ידלוק, אבל במצב של signed היינו מקבלי $-1$ שזה תקין.

## overflow flag 
משתמשים בו לחישובים של signed
הוא יידלק כאשר, ה sign bit של האופרנדים כבוי אבל של התוצאה הוא דולק , או ההפך.

$$01111111+01000000 = 10000000\rightarrow OF=1$$

### condition check
__למה צריך את ה register הזה?__
נוכל לבדוק שיוויון או אי שיוויון בין שתי registers או כתובות או מספרים, על ידי ביצוע פעולה ובדיקה ה flag registers, על ידי הפקודות הבאות:

![Pasted image 20221223233038.png|400](/img/user/Assets/Pasted%20image%2020221223233038.png)

באופן דומה יש לנו את אותם הפקודות עבור quad words.
למשל האם נרצה לבדוק האם ערכו של מספר הוא $0$

```
testl %ecx,%ecx and check if ZF=1
```

### השמה של ה flags register 
אם נרצה לבצע השמה של הפלאגים בregisters משלנו נוכל להשתמש בפקודות הבאות

![Pasted image 20221223233434.png|400](/img/user/Assets/Pasted%20image%2020221223233434.png)

נשים לב שיש כאן פעולות של above ו greater שאחת בודקת השוואת ערכים ואחת השוואה לוגית לפי הביטים.
## jumps
הפקודה jump מאפשר לנו לבצע קפיצה לlabel מסויים בצורה מותנת (לפי ה flags register) או בצורה בלתי מותנת. יש לזה חשיבות גבוהה מאוד ביצירת לולאות ותנאים שתיכף נראה.

### unconditional jump
בהינתן לייבל L נוכל באמצעות `jmp L` להזיז את ה program counter ל L ולהמשיך משם את הקוד, נשים לב שבמצב זה לא מצופה מהתוכנית לחזור לנקודה שממנה נקרא ה jmp.

ליכולת הנ״ל קוראים direct jump. נוכל לבצע גם undirect jump באמצעות register באופן הבא
1) `jmp *%rax` - קפיצה לכתובת שהיא הערך שנמצא בתוך rax. הכוונה בערך זה ה immediate 
2) `jmp *(%rax)` - ללכת לכתובת בזכרון שרשומה בתוך rax , ולקפוץ לשם.

### conditional jump
קפיצה בהתאם לערכים שנמצאים ב register flags כפי שרשום בטבלה הבאה 

![Pasted image 20221224001306.png|350](/img/user/Assets/Pasted%20image%2020221224001306.png)

במצב של קפיצה מותנת ניתן לתת רק label.
הרעיון הוא שאם נרצה למשל לבדוק תנאי, אנחנו נשתמש בפקודות האריתמטיקה שמשנות את הflag הזה, ולאחר מכן נוכל להשתמש בקוד הזה כדי לבצע scpoing בהתאם להאם התנאי התקיים או לא.

### if-else
נוכל לתאר התניות באסמבלי בצורת פסודו קוד באופן הבא
``` psuedo
t = boolean-expression;
if (t)
	goto true;
else 
	goto done;
true:
	print("true")
done:
	print("done")
```
בעצם אנחנו קופצים לשורת קוד מסויימת תוך הזנחה של כל מה שנמצא מאחורי השורה שאליה קפצנו.
__ובפועל:__
נמיר כעת את הקוד c הבא לקוד אסמבלי`
``` c
int absdiff(int x, int y) {
	if (x<y)
		return y-x;
	else 
		return x-y;
}
```
ובאסמבלי
``` assembly
# x in %edi, y in %esi
	cmpl   %esi,%edi
	jl .L3
	subl %esi, %edi
	jmp .L5
.L3:
	subl %edi, %esi
	movl %esi, %eax
.L5: 
```

### Do - While Loops
נוכל לתאר לולאות do while באסמבלי בצורת פסודו קוד באופן הבא
``` psuedo
loop:
  //some code
  t = bool;
  if (t)
	goto loop;
```
והמרה של קוד c לקוד אסמבלי
![Pasted image 20221224002659.png|450](/img/user/Assets/Pasted%20image%2020221224002659.png)

__באופן די דומה נוכל לתאר while loops__ 
``` psuedo
if(!boolean)
	goto done;
loop:
	body-statement
	t = boolean
	if (t)
		goto loop:
done:
```

למשל עבור הקוד הזה
``` c
int loop_while(int a, int b) {
 int i = 0;
 int result = a;
 while (i < 256) {
	 result += a;
	 a -= b;
	 i += b;
 }
}
```
הקוד אסמבלי ייראה כך

``` assembly
xorl  %ecx, %ecx
movl  %edi, %edx

.L5:
 addl %edi ,%edx
 subl %esi, %edi
 addl  %esi, %ecx
 cmpl $255, %ecx
 jle .L5
```

### for loops
נוכל לתאר לולאות for  באסמבלי בצורת פסודו קוד באופן הבא
``` psuedo
init-expr;
t = test-expr;
if(!t)
	goto done;
loop:
	body-statement
	update-expr;
	t = test-expr;
	if(t)
		goto loop;
done:
```
ובהמרת קוד c ל assembly
``` c
long loop(long x, long y, long n) {
	long result = 0;
	long i;
	for(i =  n - 1 , i >=0 , i = i-x) {
		result += x * y;
	}
	return result;
}
```


``` assembly
# x in rdi, y in rsi , n in rdx
  xorq %rax, %rax
  decq %rdx
  js. L4
  imulq %rdi, %rsi
.L6:
addq %rsi, %rax
subq %rdi, %rdx
jns .L6
```
נשים לב שהאופטימיזצייה כאן היא לעשות את הפעולה שעושים t פעמים מחוץ ללולאה כיוון שגם התנאי נעשה מחוץ ללולאה

### switch statements
נזכר רגע איך זה נראה ב c
``` c
switch (expression)
{
    case constant1:
      // statements
      break;

    case constant2:
      // statements
      break;
    .
    .
    .
    default:
      // default statements
}
```

באסמבלי אנחנו נממש את זה עם jump table. 
בהנחה שהcases הם מספרים שיחסית קרובים אחד לשני למשל בין 100 ל 106. נבנה את ה jump table באופן הבא
``` assembly
.section .rodata
.align 8

.L10:
.quad.L4 #case 100
.quad.L3 #case 101, not exists, sends to defult
.quad .L5 #case 102
.quad .L6 #case 103
.quad .L7 #case 104
.quad .L8 #case 105
.quad .L9 #case 106
```
הפקודה `.align` מבקשת שכל הכתובות של הערכים שבdata שנמצאים אחרי הפקודה להיות בכפולות של 8. זה לא פעולה שנתמכת על ידי רוב הקומפיילרים. באופן כללי חייבת להיות alignment מסויים למשתנים מסוגים שונים. למשל , משתנים בגודל word חייבים להיות בכתובות שמתחלקים ב 4. הקומפיילר לרוב יבצע התאמה על ידי padding של 16 במחסנית באופן דיפולטי עבור כל משתנה מקומי ועבור כל קריאה לפונקצייה שנעשה.
פקודות מהסוג הזה נקראות [directives](https://docs.oracle.com/cd/E19253-01/817-5477/eoiyg/index.html).

אם כן מה שעשינו זה לתת לכל הLabels שיצרנו כתובות שמופרדות זה בזה על בהפרש של 8 בייטים.
כעת נוכל להשתמש ב load effective address כדי לגשת לאזור המתאים 
```assembly
leaq -100(%rdi), %rsi
cmpq $6 ,%rsi #the condition that we want to check
ja .L9 #if > goto default - case
jmp *.L10(, %rsi, 8) #goto jump-table[rsi]
```
1) משתמשים ב jump above כדי שנלך לדיפולט גם עבור תוצאה שלילית
2)  הפעולה הראשונה בעצם לוקחת את מה שיש ב rdi (נניח שהוא מחזיר את האפשרות שנבחרה בתפריט) מחסר ממנה 100 ושם את התוצאה ב rsi
3) השורה האחרונה נראת מורכבת אבל מדובר ב indirect jump שקופץ ל immediate של L10 כלומר הכתובת שלו, עם הפעולה האריתמטית שהיא $mem[L10]+0+8rsi$.

זאת בעצם הפעולה שעשינו:
![Pasted image 20221224020606.png](/img/user/Assets/Pasted%20image%2020221224020606.png)

נשים לב לכמה דגשים חשובים: 
1) אם טווח המספרים הוא בין $x,x+a$ אז חייב לבנות מערך בגודל $a$ איברים. גם אם ה `switch` לא מכסה את כל    האפשרויות האלה, עדיין נבנה להן אופצייה ששולחת לאיזה דיפולט `case` או משהו כזה.
2) `.L10` היא לא פונקצייה בפני עצמה אלא label ב rodata , כלומר שהוא רק מהווה נקודת כניסה למערך.



