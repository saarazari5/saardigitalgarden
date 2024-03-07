---
{"dateCreated":"2023-09-03 16:36","tags":["operating_system","computer_science"],"pageDirection":"rtl","dg-publish":true,"permalink":"/computer-science/operating-systems/threads/","dgPassFrontmatter":true}
---


# Threads
[[Computer Science/Operating Systems/Process Management\|Process]] - מגדיר את מרחב הכתובות, טקסט, משאבים וכו׳
__Threads-__ מגדיר חלק מסויים של הרצה בתוך process (PC,Stack,Registers).

כמה דגשים חשובים על threads:
1) threads תמיד משוייכים ל process.
2) לprocess יכול להיות מספר threads שיכולים לתקשר בינהם ללא system calls, message passing או shared memory.
![Pasted image 20230903173048.png|450](/img/user/Assets/Pasted%20image%2020230903173048.png)

שימוש ב threads יעיל יותר ומאפשר סינכרוניזצייה טובה יותר בגלל שהם חולקים בינהם code, data ,files .

__היתרונות של threads:__
* _responsiveness:_ התוכנית יכולה לרוץ גם כאשר חלקים ממה חוסמים או מבצעים פעולה מאוד ארוכה.ֿ
* _resource sharing:_ תהליכונים חולקים את המידע של התהליך שיצר אותם.
* _economy:_ להחליף בין threads זאת פעולה הרבה יותר קלה מאפשר לבצע context switch בין processes.
* _scalability:_ במערכת multiprocessors יכולים מספר threads לרוץ על מספר מעבדים בו זמנית ולהריץ חלקים שונים מאותה התוכנית.

__אתגרים:__
* חלוקת משימות בין threads בצורה נקיה ומקבילית. 
* חלוקה מאוזנת בין המשימות מבחינת ניצולת מעבד. כלומר שאם thread אחד מבצע משימה והשני ממתין לו אין סיבה שהוא יקבל זמן מעבד בעצמו.
* שמירת הdata באופן כזה שיתמול בעדכונים והרצה מקביליים.
* קושי ב debugging.

## Kernel Threads
* זה thread שמערכת ההפעלה מכירה. 
* החלפה בין kernel threads מצריך context switch אם כי הוא קל יותר לביצע מהcontext switch בין תהליכים עצמם (צריך להחליך רקק ערכי registers, PC ו stack pointer).
* הkernel מתזמן בין הthreads הללו (בנוסף ל[[Scheduling\|תזמון ]]בין processes).

__היתרון__ המשמעותי של kernel threads הוא בעובדה שהוא מהווה סוג של תהליך, כלומר מערכת ההפעלה מתזמנת בינהם באותו אופן שבוא היא מתזמנת תהליכים ולכן יכולה להיות מקביליות. המחיר הוא over head של context switch. 
## User Threads
threads שנוצרים ברמת ספרייה כלשהי. הניהול שלהם הוא בעצם ההחלטה מעל איזה kernel thread ישבו ה user threads של אותה תוכנית.

__מערכת ההפעלה לא מכירה את הuser threads שנוצרים ברמת הספרייה, כלומר מבחינת מערכת ההפעלה התהליך שמריץ תוכנית שלנו הוא single thread__.  

![Screenshot 2023-09-03 at 18.33.20.png](/img/user/Assets/Screenshot%202023-09-03%20at%2018.33.20.png)

__יתרונות:__
1. אין context switch כאשר מחליפים בין threads.
2. תזמון של user threads הוא יותר גמיש:
	* ניתן לתזמן בין threads מסויימים במודול כלשהו בקוד שלי באופן מסויים ובמודול אחר לתזמן אותם אחרת. 
	* כל process יכול לנהל את הthreads שלו אחרת ועם ספרייה אחרת.
	* thread יכול לוותר על הריצה שלו ולהעביר אותה לthread אחר באמצעות `yield`.
3. אין overhead ביצירה כלומר אין צורך בsystem call כדי לאתחל user thread.
4. מהירים יותר מ kernel threads.
5. יכולים להתקיים בלי תלות במערכת ההפעלה.

__חסרונות:__
 1. אין מקביליות אמיתית שכן מערכת ההפעלה לא מכירה אותם.
 2. לא יכול לקבל החלטות תזמון שתלויות ב user level threads למשל הוא יכול להריץ תהליך שכל ה user threads שלו הם idle. או להעביר process למצב של waiting כי user thread אחד שלו מבצע I/O. הוא מתזמן processes ללא תלות במספר ה user threads שלהם.

## Threading Models
הפתרון לבעיות הנ״ל הוא בעצם שיוך של הuser threads ל kernel threads (זה קוד שהספרייה מבצעת באתחול). בשיטה זאת הספרייה יכולה להגדיר כיצד היא רוצה שהuser threads יתזומנו על ידי מערכת ההפעלה. מבנה הנתונים שמאפשר את זה נקרא [Lightweight process (LWP)](https://www.tutorialspoint.com/lightweight-process-lwp)

![Screenshot 2023-09-03 at 19.01.48.png](/img/user/Assets/Screenshot%202023-09-03%20at%2019.01.48.png)

__Many to One__:
מספר user threads ממופים ל kernel thread אחד. כלומר, לכל תהליך יש kernel thread אחד  והספרייה המנהלת תחליט בכל רגע נתון מי ה user thread שירוץ ברגע שה kernel thread יקבל זמן מעבד. הניהול הוא יעיל אבל יש חסרון משמעותי שכן לכל תהליך יש kernel thread יחיד ואנחנו מעבדים את היכולת ל paralism של התהליך בגלל זה. כמו כן עדיין יש השבתה של כל התהליך ברגע ש thread אחד נכנס ל I/O

__One to One:__
לכל user thread מוצמד kernel thread ייחודי לו. 
היתרון הגדול הוא שנוכל לקבל מקביליות אבל במחיר של over head כי מייצרים המון kernel thread עבור מערכת multithreaded.

__Many to Many:__
לכל תהליך יש pool של kernel threads והתהליך יכול להשתמש בהם בהתאם.

__Two-Level:__
שילוב של MM ו OO כך שנוכל להגן עבור user threads שעושים פעולות חשובות מפני יציאה לI/O של threads אחרים.
