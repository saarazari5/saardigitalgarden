---
{"dateCreated":"2023-01-20 01:18","tags":["computer_system","assembly"],"pageDirection":"rtl","dg-publish":true,"permalink":"/computer-science/computer-system/gdb/","dgPassFrontmatter":true}
---


# gdb
**GNU Debugger**, הידוע יותר בשמו **GDB**, הוא פשוט מנפה שגיאות הסטנדרטי למערכות התוכנה של GNU.  
כחלק מהכלים המרכזיים שלה היא מאפשרת:

א. לעצור את ריצת התוכנית בכל שלב
ב. להגדיר נקודות עצירה או תנאים שבהם התוכנית צריכה לעצור כדי שתוכל לבדוק את המצב שלה.
ג. לצפות בערכים של משתנים
ד. ריצה של התוכנית שורה אחרי שורה

## תחילת השימוש
יש לקמפל את התוכנית באופן שתומך בdebug 
```gdb
gcc -Wall -g program.c
```
ה `-g` שומר על המזהים וסימונים של משתנים ו labels.

כדי להתחיל את $GDB$ :
``` gdb
gdb a.out
```

וניתן להוסיף ארגומנטים 
``` gdb
gdb --args a.out arg1 arg2
```

## פקודות מועילות במיוחד

``` gdb
1) refresh - refresh the display

2) run - run the program

3) layout next - see your code

4) break POINT - set a break point where POINT can be a line number, function name , memory address and more.

5) delete n- delete the n-th break point

6) clear POINT - delete the break point using POINT value

7) nexti- step to next line

8) stepi- step to next line but jump into a function when stepping on one.

9) continue- step to next breakpoint

10) print VARIABLE - print a variable

11) print *arr@len - print an array

12) watch VARIABLE - watch variable change

13) layout asm - This command is used to display the source code and assembly code side by side.

14) x/i - This command is used to examine memory and display the contents as assembly instructions.

15) context stack- present the stack at a give point in the program

16) function info breakpoint- give all the breakpoint on function

17) r- restart the program with the same old breakpoint

18) quit- quit gdb

19) i b- see all breakpoints 
```

## קבלת מידע על פונקציות 
על ידי שימוש בפקודה `info function` נוכל לקבל רשימה של כל הפונקציות שנמצאות בקוד ב symbol table.

### disassembly in gdb
נוכל גם לבצע דיסאסמבלי [[Computer Science/Computer System/Disassembly - basics\|Disassembly - basics]] לפונקצייה מסויימת על ידי הפקודה  `disassembly main` .

## מעקב אחר זכרון 
הפקודה 
```gdb
x/[format][count]address
```
היא פורמט לפקודה של בדיקת זכרון.
ה $[format]$ קובע באיזה ייצוג יש להראות את המידע (לרוב נשתמש ב hex).
ה $[count]$ קובע כמה ערכים נרצה לראות החל מהכתובת שניתן לו ב address.
__ניתן להחליף את address גם בregister שבתוכו יש כתובת כלשהי__.

למשל הפקודה x/10x 0x4005 תציג 10 בייטים מהזכרון החל מהכתובת 0x4005 בפורמט hex.
x/5d $rbp ייתן בפורמט דסימלי את 5 הערכים החל מהכתובת שנמצאת ב rbp.

__ניתן לבצע אריתמטיקת כתובות כדי לחשב כתובת מסויימת עם פעולות חיבור חיסור וכפל למשל__

```gdb
 x/d ($rsp + 0xc)
```

נשים לב לפורמט `x/i`  שמאפשר לנו לצפות בפקודות אסמבלי בפורמט הדומה לפורמט למעלה. במקום לראות ערכים לראות פקודות כאשר הכתובת היא לtext segment לשורה מסויימת .
למשל 
```c
int main() {
    int x = 5;
    int y = 6;
    int z = x + y;
    return 0;
}
```
עבור הקוד הנ״ל אם נריץ `x/5i $pc` כאשר עצרנו בשורה הראשונה של main נקבל משהו בסגנון

```assembly
=> 0x4004c6 <main+0>:    push   %rbp
   0x4004c7 <main+1>:    mov    %rsp,%rbp
   0x4004ca <main+4>:    movl   $0x5,-0x4(%rbp)
   0x4004d1 <main+11>:   movl   $0x6,-0x8(%rbp)
   0x4004d8 <main+18>:   mov    -0x4(%rbp),%eax
```

פורמט נוסף שחשוב להכיר הוא `x/s` שאומר לgdb לבדוק ערך של זכרון מסויים ולהציג את הערך כ null terminated string . נשים לב שהפקודה תעבוד רק אם הערך בכתובת הוא אכן מחרוזת שכזו. והוא ידפיס עד התו `/0` הראשון שהוא רואה.

למשל עבור הקוד 

```c
#include <stdio.h>

int main() {
    char message[] = "Hello, World!";
    printf("%s\n", message);
    return 0;
}

```

הפעלת הפקודה `x/s message` יציג משהו כזה 

```assembly
0x601048: "Hello, World!"
```

כמו כן בדומה לפורמט שהצגנו למעלה ניתן גם להגביל את מספר התווים על ידי הוספת count flag.

## layout
ניתן במהלך ריצה או לפני ריצה להציג את הקוד בחלונות על גבי התוכנה שעליה מריצים את הgdb זאת על מנת להבין יותר טוב מה קורה בתוכנית בתצורה כמו למשל 

![Pasted image 20230120154510.png](/img/user/Assets/Pasted%20image%2020230120154510.png)

כמה פקודות חשובות 


``` gdb
-   "layout split": This command splits the gdb window horizontally, creating a top and bottom window. You can use the command "focus up" and "focus down" to switch between the two windows.
    
-   "layout src": This command opens the source code in a separate window. You can navigate through the source code, set breakpoints, and step through it.
    
-   "layout asm": This command opens the assembly code in a separate window. You can navigate through the assembly code, set breakpoints, and step through it.
    
-   "layout reg": This command opens the registers window. You can see the values of the registers, change them and step through the assembly code.
    
-   "layout next": This command cycles through the available layouts.
    
-   "layout save": This command saves the current layout, you can give it a name and use it later with "layout restore" command
    
-   "layout restore": This command restores a layout that was previously saved
    
-   "layout" : This command shows the current layout

- refresh - Refresh the screen. This is similar to typing C-L.

```

שימוש בפקודה `laynext` ישים חלון עם קוד אסמבלי ולאחר מכן ניתן להשתמש בפקודה `list` כדי לראות את קוד המקור. ובאמצעות פקודות הlayout ניתן לשים אותם אחד ליד השני.

## help 
נוכל להשתמש בפקודה `gdb help` כדי לקבל מידע נוסף שמגיע ישירות מהדוקומונטצייה ככה למשל אם נרצה לדעת עוד על הפקודה  `x` נוכל לרשום :

```GDB
(gdb) help x
Examine memory: x/FMT ADDRESS.
ADDRESS is an expression for the memory address to examine.
FMT is a repeat count followed by a format letter and a size letter.
Format letters are o(octal), x(hex), d(decimal), u(unsigned decimal),
  t(binary), f(float), a(address), i(instruction), c(char) and s(string),
  T(OSType), A(floating point values in hex).
Size letters are b(byte), h(halfword), w(word), g(giant, 8 bytes).
The specified number of objects of the specified size are printed
according to the format.

Defaults for format and size letters are those previously used.
Default count is 1.  Default address is following last thing printed
with this command or "print".
```