---
{"dateCreated":"2024-02-17 17:28","tags":["concurrency"],"pageDirection":"rtl","dg-publish":true,"permalink":"/computer-science/algorithms/parallel-algorithms/parallel-sort/","dgPassFrontmatter":true}
---

# Parallel Sort
אנחנו מכירים [[Computer Science/Algorithms/Sorting Algorithms\|אלגוריתמי מיון סדרתיים]] וגם שחסם המיון שלהם כאשר היחס היחיד שמכירים הוא יחס הסדר הוא $O(n\log n)$ . 

$n$ מעבדים יכולים להקטין את זמן הריצה לכל היותר פי $n$, בתנאי שהם רצים במקביל, ובתנאי שאין המתנה של מעבד אחד לשני. אם אכן יהיה לנו מעבד לכל ערך וההנחות הנ״ל מתקיימות זמן הריצה יצטמצם במקרה אופטימלי ל $O\left(  \frac{n\log n}{n} \right)= O(\log n)$ .

למרות זאת חשוב לציין כי ההנחה שיש לנו מעבד עבור כל ערך אינה מציאותית. לרוב, מעבד אחד יקבל אוסף של ערכים לעבוד עליהם.

כדי להבין איך נגשים לבעיית המיון המקבילי ננסה לחשוב איך אפשר למצוא את ה$min$ במערך לא ממוין. הפתרון ההגיוני הוא לחלק את המערך ל $\frac{n}{2}$ זוגות ולהצמיד לכל זוג $\text{Thread}$ , זה ישווה בין הזוגות וכעת $\frac{n}{2}$ תהליכונים מחזיקים בתוכם את ה$min$ מבין כל זוג. ניתן כעת לחלק את ה $\frac{n}{2}$ ערכים שלנו ל $\frac{n}{4}$ זוגות ולבצע את אותו תהליך עם $\frac{n}{4}$ תהליכים וכן הלאה... 
כל פעולת השוואה היא פעולה בזמן קבוע $O(1)$. תחת ההנחה שה$\text{Threads}$ פועלים במקביל בלי המתנה שלהם אחד לשני, פעולת ההשוואה הזאת תבוצע $O(\log n)$ פעמים.

בעצם הרעיון הוא __חשיבה מקבילית__ מדובר פה גם בחלוקת משאבים (תהליכונים) בצורה נכונה כדי שהאלגוריתם באמת יעבוד בצורה יעילה יותר. בשלב הראשון נלקח אלגוריתם סדרתי יעיל ונבין איך ניתן (אם ניתן) לחלק את המשאבים כדי לקבל אלגוריתם מקבילי יעיל יותר. גם נרצה להבין מהו החסם האופטימלי שנקבל כדי לדעת האם שווה בכלל להקצות משאבים ומאמץ להמרה של האלגוריתם לצורה מקבילית.

## Bubble Sort
במיון בועות, המספר הגדול ביותר מועבר תחילה לסוף הרשימה על ידי סדרה של פעולות השוואה והחלפה. ההליך חוזר על עצמו, ועוצר ממש לפני המספר הגדול ביותר שהוצב קודם לכן, כדי לקבל את המספר הבא בגודלו. בדרך זו, המספרים הגדולים יותר נעים (כמו בועה) לקראת סוף הרשימה. זמן הריצה הוא $O(n^{2})$

![Pasted image 20240217214657.png](/img/user/Assets/Pasted%20image%2020240217214657.png)

הקוד הסדרתי נראה כך:

![Pasted image 20240217214719.png|350](/img/user/Assets/Pasted%20image%2020240217214719.png)

__האם ניתן למקבל את האלגוריתם?__
מיון בועות הוא אלגוריתם __סדרתי__ בלבד. כל שלב בלולאה הפנימית מתרחש לפני הבא, וכל הלולאה הפנימית הושלמה לפני האיטרציה הבאה של הלולאה החיצונית. עם זאת, רק בגלל שהקוד הסדרתי משתמש בהצהרות התלויות בהצהרות קודמות לא אומר שלא ניתן לנסח אותו מחדש כאלגוריתם מקביל. 
==פעולת הבעבוע של האיטרציה הבאה של הלולאה הפנימית יכולה להתחיל לפני שהאיטרציה הקודמת הסתיימה, כל עוד היא לא תעקוף את פעולת הבעבוע הקודמת.== 

__פתרון 1__
הפתרון הראשון שנציע כדי לממש את bubble sort באופן מקבילי הוא קונספט שמקובל בתחומים שונים של מדעי המחשב שהמטרה שלו הוא למנוע המתנה מיותרת כדי להתחיל איזשהו תהליך. הקונספט הזה נקרא __Pipeline__ והוא יבוא לידי ביטוי באלגוריתם הזה בכך שלא נצטרך לחכות לאיטרציה פנימית שתסתיים כדי להתחיל כבר את האיטרציה הבאה. 

$𝑃_0$ מתחיל מיון ע"י השוואה של $a[0]$ עם $a[1]$. לאחר מכן $𝑃_0$ ממשיך להשוות $a[1]$ עם $a[2]$. מעכשיו, $𝑃_1$ יכול להיכנס לתמונה - $𝑃_0$ משווה $a[2]$ עם $a[3]$, יחד עם $𝑃_1$ שמשווה בין $a[0]$ ו- $a[1]$. ככה המעבדים מבצעים פעולות במקביל ובעצם חוסרים את ההמתנה כל איטרציה פנימית כדי להתחיל את ההבאה בתור.

![Pasted image 20240217221413.png|400](/img/user/Assets/Pasted%20image%2020240217221413.png)

מבחינת זמן הריצה: כשThread הראשון מגיע לסוף המערך נכנס הThread ה $\frac{n}{2}$ , וכשהוא יגיע לסוף המערך יגיע ה Thread ה $n$ כלומר בשתי איטרציות על המערך נסיים את המיון. סך הכל זמן הריצה הוא $O(n)$ .

כמובן שצריך להתחשב בבניית האלגוריתם הזה בהקצאת משאבים לסנכרון, למשל אם נרצה ש $P_{0}$ יודיע ל$P_{1}$ שהוא יכול להיכנס נוכל להשתמש במנגנון כמו $semaphore$ שלא גורם ל _busy waiting_ (זה מצב שבו יש המתנה של תהליך על זמן מעבד ולא ב I/O).

### Odd-Even Sort
וריאציה נוספת של מיון בועות הנקראת מיון אי זוגי-זוגי (טרנספוזיציה), הפועלת בשני שלבים מתחלפים, שלב זוגי ושלב אי זוגי. בשלב הזוגי, תהליכים זוגיים מחליפים מספרים עם שכניהם משמאל. באופן דומה, בשלב האי-זוגי, תהליכים אי-זוגיים מחליפים מספרים עם שכניהם הימניים.

![Pasted image 20240217222505.png|400](/img/user/Assets/Pasted%20image%2020240217222505.png)

אז ניתן לראות כאן שבשלב ה$0$ שהוא זוגי. הזוגות הם $\{(P_{0},P_{1}),(P_{2},P_{3})\dots{}\}$ ובשלבים האי זוגיים $1$ וכו׳ הזוגות הם $\{(P_{1},P_{2}),(P_{3},P_{4})\}$ והם מבצעים בינהם החלפות.

דוגמת הרצה נוספת: 

![Pasted image 20240217222941.png|250](/img/user/Assets/Pasted%20image%2020240217222941.png)

האלגוריתם ב C יראה מהצורה:
```c
rank = process_id();
A = initial_value(rank);
for (i = 0; i < N; i++) {
    if (i % 2 == 0) { // even phase
        if (rank % 2 == 0) { // even process
            recv(&B, rank + 1); send(A, rank + 1);
            A = min(A,B);
        }  
        else { // odd process
            send(A, i - 1); recv(&B, i - 1);
            A = max(A,B);
         }
     } 
     else if (i > 0 && i < N - 1) { // odd phase
                  if (i % 2 == 0) { // even process
                       recv(&B, rank - 1); send(A, rank - 1);
                       A = max(A,B);
                   } else { // odd process
                   send(A, i + 1); recv(&B, i + 1);
                   A = min(A,B);
      }
}
set_final_value(A, rank);
```

_זמן הריצה:_ 
מיון הבועות המקבילי יבצע n איטרציות של הלולאה הראשית, ולכל איטרציה יתבצעו במקביל השוואות $n-1$ ו $\frac{n-1}{2}$ החלפות. אם מספר המעבדים הוא $n$, רמת המורכבות של הלולאות הפנימיות היא $O(1)$ מכיוון שכל האיטרציות מבוצעות במקביל. הלולאה החיצונית תתבצע במשך $n$ פעמים לכל היותר. כתוצאה מכך, המורכבות של מיון בועות מקביל שווה ל-$O(n)$. 

## מדדים 
נשאלת השאלה כיצד אפשר למדוד את יעילות הביצועים. ניתן להציע כמה אופציות לשאלה הזאת (נדגים מדדים על האלגוריתם הנ״ל של מיון בועות מקבילי)

__Scalability__
כיצד משתפר הspeedup של האלגוריתם ככל שמוסיפים מעבדים : $\text{speedup} = \frac{\text{Time}_{serial }}{\text{Time}_{parallel}}$

![Pasted image 20240217232320.png|400](/img/user/Assets/Pasted%20image%2020240217232320.png)

ניתן לראות שהחל ממספר מסויים של מעבדים היעילות יורדת כי הoverhead של לייצר אותם כבר לא משתלם.

__Parallel Cost__
העלות של זמן הריצה המקבילי במספר המעבדים $\text{Parallel Cost}=\text{Time}_{\text{parallel}}\cdot \text{number of processors}$

![Pasted image 20240217232518.png|450](/img/user/Assets/Pasted%20image%2020240217232518.png)
נקודת שפל בשמונה מעבדים שקיבלנו זה בגלל שיש קפיצות התייעלות משמעותיות עד שמגיעים להשתמש ב- 8 מעבדים, ומשם יש תועלת קטנה יותר משמעותית לתוספת מעבדים.

__Parallel Efficiency__
היחס בין הspeedup למספר המעבדים 

$$\text{ Parallel Efficiency}= \frac{\text{speedup}}{\text{number of processors}} $$

![Pasted image 20240217232721.png|450](/img/user/Assets/Pasted%20image%2020240217232721.png)

ניתן לראות שהיעילות המקבילית יורדת ככל שמוסיפים עוד מעבדים המשמעות היא שהשיפור ב speedup ככל שמוסיפים מעבדים לא באמת מוסיף הרבה.

__runtime__
זמן הריצה
![Pasted image 20240217232825.png](/img/user/Assets/Pasted%20image%2020240217232825.png)

![Pasted image 20240217232835.png|450](/img/user/Assets/Pasted%20image%2020240217232835.png)
החל מ- 64 מעבדים יש הרעה. למה ? זה יכול לנבוע מחלוקה עדינה מדי (כלומר, לחלקים קטנים מדי) של המערך שלנו בין המעבדים.

כדי להגדיר את המונחים עבור חלוקה עדינה של מעבדים או חלוקה גסה של מעבדים נגדיר שני מושגים:

__Fine-grain parallelism:__ תוכנית מפוצלת להרבה משימות קטנות. כל משימה מוקצית למעבד
א)  Granularity נמוך.
ב) התקורה גבוהה (תקשורת, סנכרון, ניהול תהליכים, גישה לזיכרון לעומת חישובים)

__Coarse-grain parallelism:__ תוכנית מפוצלת למספר משימות גדולות
כמות גדולה של חישוב מתרחשת במעבדים ללא צורך בתקשורת עם מעבדים אחרים
א) Granularity גבוה
ב) תקורה נמוכה


__Granularity:__ זה היחס בין זמן החישוב לבין זמן תקשורת של מעבדים אחד עם השני $\frac{\text{computation time}}{\text{communication time}}$

עבור האלגוריתם של מיון בועות מקבילי רואים שכאשר משתמשים בשני מעבדים, אז ה- overhead עבור תקשורת בין המעבדים וסנכרון בין המעבדים הינו נמוך, ורוב זמן ריצה הוא של חישובים, שזה מצב טוב

![Pasted image 20240217234818.png|350](/img/user/Assets/Pasted%20image%2020240217234818.png)

## Merge Sort
אלגוריתם שהוא אינו in-place ומבצע חלוקה ריקורסיבית של המערך עד שמגיעים לאיברים יחידים לאחר מכן מחברים את המערכים ומחברים בינהם בחזרה כאשר הפלט הוא מערך ממויין. אומנם האלגוריתם לא יעיל מבחינת זכרון אבל הוא מבטיח זמן ריצה של $O(n\log n)$ שאנחנו יודעים שזה החסם התחתון על מיון מבוסס השוואות.

![Pasted image 20240217234953.png|400](/img/user/Assets/Pasted%20image%2020240217234953.png)

בתור חשיבה ראשונית היינו יכולים לעשות שכל ההשוואות והפיצולים יהיו בצורה מקבילית והחלק של הmerge יהיה בצורה סדרתית. במצב כזה אבל נקבל זמן ריצה של $O(n)$ כי כדי למזג שני רשימות בגודל $i$ נדרש $2i-1$ צעדים במקרה הגרוע. בגלל הפיצולים נבצע את המיזוגים האלה $\log n$ פעמים וסך הכל זמן הריצה יהיה $\sum\limits_{i=1}^{\log n} 2^i  -1\in O(n)$

![Pasted image 20240218165800.png](/img/user/Assets/Pasted%20image%2020240218165800.png)

### Odd Even Merge
זהו אלגוריתם מקבילי לביצוע מיזוג עבור רשימות ממוינות. הרשימות הממוינות נוצרו __באופן רקורסיבי__ על ידי שימוש באותו אלגוריתם. 

נסתכל על הדוגמה הבאה:

![Pasted image 20240218191023.png|350](/img/user/Assets/Pasted%20image%2020240218191023.png)


כל תהליך מקבל מערך עם האינדקסים הזוגיים של המערך או האינדקסים האי זוגיים במערך. לבסוף מתקבל מערך ״semi-sorted" כלומר מערך ממוזג שהאינדקסים הזוגיים ממויינים בפני עצמם וגם האינדקסים האי זוגיים ממוינים בפני עצמם. בקוד סדרתי היינו רצים על כל זוגות האיברים ומבצעים החלפות לפי הצורך. בקוד מקבילי נעשה זאת בזמן _קבוע_.

לבסוף בשלב המיזוג כל תהליך מקבל זוג איברים סמוכים, מבצע את הנוסחה שרשומה בפסודו קוד למטה כדי לדעת האם יש להחליף בין המיקומים שלהם. סך הכל זמן הריצה הוא $O(\log n)$  שכן נוסחת הנסיגה היא:
$T(n)=2\cdot T\left( \frac{n}{2} \right)+ O(1)$ 
הסיבה שזה $O(1)$ היא שעכשיו תהליך ההשמה באינדקס המתאים (באמצעות min, max) נעשה בצורה מקבילית.

![Pasted image 20240218193512.png](/img/user/Assets/Pasted%20image%2020240218193512.png)

__Odd Even Merge Sort__
כעת נסביר איך לממש את אלגוריתם ה merge sort בצורה מקבילית. אנחנו נשתמש בצורה רקרוסיבית ב Odd-Even Merge. זמן הריצה של האלגוריתם הזה יהיה $T(n)=2\cdot T\left( \frac{n}{2} \right)\cdot O(\log n)= O(\log^{2} n)$ . שכן , אנחנו מפצלים את מערך $\log n$ פעמים ופעולת המיזוג בכל שלב לוקחת $\log n$ .

![Pasted image 20240218193818.png](/img/user/Assets/Pasted%20image%2020240218193818.png)

### Bitonic Merge Sort 
הבסיס של אלגוריתם ביטוני הוא bitonic sequence. רצף ייקרא bitonic אם הוא מכיל שני רצפים, אחד עולה ואחד יורד כך ש :

$$a_{1}<a_{2}\dots < a_{i-1} > a_{i+1}>a_{i+2}\dots{> a_{n}}$$

__או__ 

$$a_{1}>a_{2}\dots > a_{i-1} < a_{i+1}<a_{i+2}\dots{< a_{n}}$$

![Pasted image 20240218194506.png|250](/img/user/Assets/Pasted%20image%2020240218194506.png)

__תכונה מיוחדת__ של רצף ביטוני הוא שאם נבצע פעולות השוואה והחלפה של האלמנטים $a_{i}$ ו $a_{i + \frac{n}{2}}$ לכל $i\in[0,n]$ אנחנו נקבל שני רצפים ביטונים כך שכל הערך ברצף אחד קטנים מהערכים של האחר. 

![Pasted image 20240218200639.png|300](/img/user/Assets/Pasted%20image%2020240218200639.png)

המשמעות היא שאנחנו יכולים __לבצע את פעולות ההחלפה וההשוואה באופן רקרוסיבי__ עד שמקבלים מערך ממוין

![Pasted image 20240218201940.png|350](/img/user/Assets/Pasted%20image%2020240218201940.png)

כעת נשאלת השאלה, כיצד אפשר להמיר כל מערך למערך מהצורה של bitonic sequence. כדי לעשות זאת נוכל לחלק את המערך לזוגות כך שזוגות באינדקס זוגי נסדר אותם להיות רצף ביטוני וזוגות באינדקס אי זוגי נסדר אותם להיות כרצף ביטוני הפוך. באופן רקרוסיבי נמזג את הזוגות האלה כך שיווצרו לנו רצפים ביטונים גדולים פי 2 עד שנקבל שני רצפים ביטונים כל אחד מהם חצי מגודל המערך ונפעיל bitonic merge כדי לקבל מערך ממויין.

![Pasted image 20240218202817.png](/img/user/Assets/Pasted%20image%2020240218202817.png)

נדגים ריצה על המערך $(3,7,4,8,6,2,1,5)$ :
	_א._ נמיר לזוגות איברים כפי שאמרנו $(6,2),(4,8)(3,7),(1,5)$. כעת נדאג שבמקומות הזוגיים ובמקומות האי זוגיים יהיה ביטוני וביטוני הפוך בהתאמה.
	_ב._ כעת נמזג בין הזוגות עם bitonic merge כדי לקבל 2 זוגות בגודל 4 של סדרות ביטוניות - $(3,7,8,4),(2,6,5,1)$ . עשינו זאת על ידי שימוש בתכונה שתיארנו מקודם. נמזג פעם נוספת כדי לקבל סדרה ביטונית אחת גדולה $(3,4,7,8,6,5,2,1)$ וכעת אפשר לבצע את האלגוריתם המיזוג הביטוני על המערך הגדול כדי לקבל מערך ממויין כפי שתיארנו.

![Pasted image 20240218204828.png|400](/img/user/Assets/Pasted%20image%2020240218204828.png)


זמן הריצה הוא: $O(\log^{2}n)$. שכן, יש $\log n$ פאזות לאלגוריתם ובכל פאזה $i$ מבצעים $i$ פעולות מקביליות של מיזוג ו swap&exchange.  סך הכל נקבל $\sum\limits_{i=1}^{\log n} \frac{\log n(\log n)}{2}= O(\log^{2}n)$

__במבחן המציאות__
אם נריץ bitonic sort מקבילי, כאשר נייצר שני threads אחד לכל פעולה ריקורסיבית של bitonic merge אנחנו נקבל overhead גבוה מאוד של Threads. 

_האלגוריתם הסדרתי:_
```c
// ----------------- Bitonic Sort -----------------

void swap(int *a, int *b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

void compare_and_exchange(int *arr, int i, int j, int dir) {
    if (dir == (arr[i] > arr[j])) {
        swap(&arr[i], &arr[j]);
    }
}

/*
 * Given a bitonic sequence, 
 * x(1) < x(2) < ... < x(n/2) > x(n/2+1) > ... > x(n)
 * Sort the sequence in the given order.
 * Note: implemnetation assumes that the sequence is of length 2^k
 */
void bitonic_merge(int *arr, int start, int len, int dir) {
    if (len > 1) {
        int k = len / 2;

        for (int i = start; i < start + k; i++) {
            compare_and_exchange(arr, i, i + k, dir);
        }

        bitonic_merge(arr, start, k, dir);
        bitonic_merge(arr, start + k, k, dir);
    }
}

void bitonic_sort_rec(int *arr, int start, int len, int dir) {
    if (len > 1) {
        int k = len / 2;

        bitonic_sort_rec(arr, start, k, 1);
        bitonic_sort_rec(arr, start + k, k, 0);
        bitonic_merge(arr, start, len, dir);
    }
}

void bitonic_sort(int *arr, int len) {
    bitonic_sort_rec(arr, 0, len, 1);
}
```

_האלגוריתם המקבילי:_
```c

// ----------------- Bitonic Sort -----------------

void swap(int *a, int *b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

void compare_and_exchange(int *arr, int i, int j, int dir) {
    if (dir == (arr[i] > arr[j])) {
        swap(&arr[i], &arr[j]);
    }
}

typedef struct merge_args {
    int *arr;
    int start;
    int len;
    int dir;
} merge_args;

/*
 * Given a bitonic sequence, 
 * x(1) < x(2) < ... < x(n/2) > x(n/2+1) > ... > x(n)
 * Sort the sequence in the given order.
 * Note: implemnetation assumes that the sequence is of length 2^k
 */
void *bitonic_merge(void *arg) {
    merge_args *args = (merge_args *) arg;
    int start = args->start;
    int len = args->len;
    int dir = args->dir;

    if (len > 1) {
        int k = len / 2;

        for (int i = start; i < start + k; i++) {
            compare_and_exchange(args->arr, i, i + k, dir);
        }

        pthread_t tid1, tid2;
        merge_args args1 = {args->arr, start, k, dir};
        merge_args args2 = {args->arr, start + k, k, dir};

        pthread_create(&tid1, NULL, bitonic_merge, &args1);
        pthread_create(&tid2, NULL, bitonic_merge, &args2);

        pthread_join(tid1, NULL);
        pthread_join(tid2, NULL);
    }
}

typedef struct sort_args {
    int *arr;
    int start;
    int len;
    int dir;
} sort_args;

void *bitonic_sort_rec(void *arg) {
    sort_args *args = (sort_args *) arg;
    int start = args->start;
    int len = args->len;
    int dir = args->dir;

    if (len > 1) {
        int k = len / 2;

        pthread_t tid1, tid2;
        sort_args args1 = {args->arr, start, k, 1};
        sort_args args2 = {args->arr, start + k, k, 0};

        pthread_create(&tid1, NULL, bitonic_sort_rec, &args1);
        pthread_create(&tid2, NULL, bitonic_sort_rec, &args2);

        pthread_join(tid1, NULL);
        pthread_join(tid2, NULL);

        merge_args merge_args = {args->arr, start, len, dir};
        bitonic_merge(&merge_args);
    }
}

void bitonic_sort(int *arr, int len) {
    sort_args args = {arr, 0, len, 1};
    bitonic_sort_rec(&args);
}
```

יש לנו פה עץ אקספוננציאלי של תהליכים, ברגע שהילדים מסיימים לבצע bitonic_merge האבא מפעיל בעצמו bitonic_merge. בעולם התיאורטי זה יכל לעבוד אבל במבחן המציאות כיוון שאין באמת מחשב שמחזיק כל כך הרבה מעבדים שמאפשרים עבודה במקביל יש פה המון overhead והמון [[Computer Science/Operating Systems/Process Management\|context switch]] מה שיגרום לזמן ריצה גרוע ביותר בהשוואה לאלגוריתם סדרתי.

![Screenshot 2024-02-19 at 0.33.54.png](/img/user/Assets/Screenshot%202024-02-19%20at%200.33.54.png)


_כיצד מטפלים בבעיה?_
	_א._ נשאיר את החלק של _המיזוג_ סדרתי.
	ב. נשתמש ב Thread Pool או לפחות נגביל את כמות הThreads.
	ג. נשתמש ב Parallel Threshold - נשתמש ב Threads בשביל bitonic_merge רק עבור מערכים מעל גודל מסויים. 

כדי לייעל את האלגוריתם, ראשית, נשתמש בשיטת [[Computer Science/Algorithms/Dynamic Programming#Bottom-up approach\|Bottom up]] במקום ב Top down. כלומר לא נפרק את המערך הגדול באופן ריקרוסיבי ונמזג אלא מהתחלה נעבוד על החלקים הקטנים ביותר של המערך ונעלה למעלה.
השיטה הזאת תאפשר לנו לעבוד בקוד מבוסס לולאות ולא רקורסיה. 

_הקוד הסדרתי:_
```c
// ----------------- Bitonic Sort -----------------

void swap(int *a, int *b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

void compare_and_exchange(int *arr, int i, int j, int dir) {
    if (dir == (arr[i] > arr[j])) {
        swap(&arr[i], &arr[j]);
    }
}

void bitonic_merge(int *arr, int start, int len, int dir) {
    if (len > 1) {
        int k = len / 2;

        for (int i = start; i < start + k; i++) {
            compare_and_exchange(arr, i, i + k, dir);
        }

        bitonic_merge(arr, start, k, dir);
        bitonic_merge(arr, start + k, k, dir);
    }
}

void bitonic_sort(int *arr, int len) {
    for (int i = 2; i <= len; i *= 2) {
        int dir = 1;
        for (int j = 0; j < len; j += i) {
            bitonic_merge(arr, j, i, (dir++) % 2);
        }
    }
}
```
במימוש זה הדבר היחיד שמשתנה זה ה bitonic_sort (מניחים שגודל המערך הוא חזקה של $2$) כעת עולים למעלה מהעלים במערך כשהכיוונים של כל זוג מתחלפים לפי הערך dir, מתחילים מלמזג מערכים בגודל 2 אחר כך בגודל 4 וכן הלאה.

_הקוד המקבילי:_
```C
#define NUM_THREADS 4
#define PARALLEL_THRESHOLD 8192

// ----------------- Bitonic Sort -----------------

void swap(int *a, int *b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

void compare_and_exchange(int *arr, int i, int j, int dir) {
    if (dir == (arr[i] > arr[j])) {
        swap(&arr[i], &arr[j]);
    }
}

typedef struct merge_args {
    int *arr;
    int start;
    int len;
    int dir;
} merge_args;

/*
 * Given a bitonic sequence, 
 * x(1) < x(2) < ... < x(n/2) > x(n/2+1) > ... > x(n)
 * Sort the sequence in the given order.
 * Note: implemnetation assumes that the sequence is of length 2^k
 */
void *bitonic_merge(void *arg) {
    merge_args *args = (merge_args *) arg;
    int *arr = args->arr;
    int start = args->start;
    int len = args->len;
    int dir = args->dir;

    if (len > 1) {
        int k = len / 2;

        for (int i = start; i < start + k; i++) {
            compare_and_exchange(arr, i, i + k, dir);
        }

        merge_args args1 = {arr, start, k, dir};
        merge_args args2 = {arr, start + k, k, dir};
        bitonic_merge(&args1);
        bitonic_merge(&args2);
    }
}

void bitonic_sort(int *arr, int len) {
    pthread_t threads[NUM_THREADS];
    merge_args args[NUM_THREADS];

    for (int i = 2; i <= len; i *= 2) {
        int dir = 1;

        if (i >= PARALLEL_THRESHOLD) {
            for (int j = 0; j < len; j += i * NUM_THREADS) {
                for (int k = 0; k < NUM_THREADS && j + k * i < len; k++) {
                    args[k] = (merge_args) {arr, j + k * i, i, (dir++) % 2};
                    pthread_create(&threads[k], NULL, bitonic_merge, &args[k]);
                }

                for (int k = 0; k < NUM_THREADS && j + k * i < len; k++) {
                    pthread_join(threads[k], NULL);
                }
            }
        } else {
            for (int j = 0; j < len; j += i) {
                merge_args args = {arr, j, i, (dir++) % 2};
                bitonic_merge(&args);
            }
        }
    }
}
```

ניתן לראות שבאלגוריתם זה המיקבול קורה רק כאשר גודל המערך גדול מ PARALLEL_THRESHOLD. כמו כן הגבלנו גם את מספר הthreads שניתן להרים בכל רגע נתון. צריך להזהר ממזל של deadlock כשעובדים עם כמות threads חופשית בלי thread pool שכן אין פה מנגנון ניהול מסודר ואם thread אחד מסיים צריך לחכות לו בלולאה נפרדת, יכול להיות מצב שיגמרו לנו הthreads שהקצנו אבל צריך עוד כדי להמשיך באלגוריתם ואז נתקע.

![Pasted image 20240219005248.png](/img/user/Assets/Pasted%20image%2020240219005248.png)
נשים לב שהזמן ריצה יותר מהיר הפעם אבל ה user time די זהה. הסיבה לכך היא ש user time מודד את זמן המעבד שהתוכנית רצה ב user space כלומר הוא סוכם את כל העבודה של המעבדים השונים לזמן אחד. ה real time הוא מה שקובע וזה זמן הריצה בפועל של התוכנית. 

## QuickSort
[[Computer Science/Algorithms/probability algorithms basics\|אלגוריתם הסתברותי]] שבונה על בחירת pivot במיקום טוב ( שהאיבר בו מקיים שהאינדקס המתאים שלו הוא בסביבת אמצע המערך) וממיין את כל האיברים סביבו באופן רקורסיבי. זמן הריצה שלו בממוצע הוא $O(n \log n)$ ו $O(n^{2})$ הוא במקרה הגרוע.

![Pasted image 20240219054118.png](/img/user/Assets/Pasted%20image%2020240219054118.png)

 
הרעיון המקבילי כאן הוא פשוט אם כי נקבל זמן ריצה גרוע מהאלגוריתמים הקודמים של $O(n)$. כל חלוקה של המערך ותתי המערכים תבוצע על ידי מעבד נפרד. 

![Pasted image 20240218212355.png](/img/user/Assets/Pasted%20image%2020240218212355.png)

בהנחה שכל חלוקה נעשית במקביל נקבל שמספר הצעדים הוא סדרה חשבונית 

$$n+\frac{n}{2}+ \frac{n}{4} + \dots = O(n)$$

כבר בדוגמה הנ״ל ניתן לראות בעיה נוספת של מקביליות ב QuickSort והיא ה load balance- יכול להווצר מצב שמעבד אחד מקבל מעט עבודה לעומת מעבד אחר שמקבל הרבה יותר עבודה.

באופן אידיאלי נרצה של המעבדים ירוצו באופן שווה כלומר יקבלו משימות שקולות מבחינת מורכבות. הקונספט הזה נקרא [[Computer Science/System Design/Scaleability\|load balancing]] 

![Screenshot 2024-02-18 at 21.28.07.png](/img/user/Assets/Screenshot%202024-02-18%20at%2021.28.07.png)

אפשר לחלק את הload balancing לשני סוגים עיקריים 
dynamic load balancing - המשימות מוקצות לתהליכים במהלך ריצת התוכנית.

centralized dynamic load balancing - תהליך המאסטר מחזיק אוסף של משימות לביצוע . המשימות מתקבלות ל תהליך בן וכשהוא מסיים אותה הוא מבקש משימה נוספת מתהליך המאסטר. זאת מכניקה מאוד מוכרת בתכנות מקבילי שנקראת work-pool. 


![Pasted image 20240218213035.png](/img/user/Assets/Pasted%20image%2020240218213035.png)


במקרה של Quicksort משימה יכולה להיות sublist כלשהו. 

![Screenshot 2024-02-18 at 21.32.01.png](/img/user/Assets/Screenshot%202024-02-18%20at%2021.32.01.png)


## סיכום למיון מקבילי מבוסס השוואות 

| algorithm          | Sequential | Parallel       |
| ------------------ | ---------- | -------------- |
| Bubble Sort        | $O(n^2)$   | O(n)           |
| Mergesort          | O(nlog(n)) | O(n)           |
| Quicksort          | O(nlog(n)) | O(n)           |
| Odd-Even Mergesort |            | $O(\log^2(n))$ |
| Bitonic Mergesort  |            | $O(\log^2(n))$ |

## Count sort
[[Computer Science/Algorithms/Sorting Algorithms#count sort\|מיון מניה]] הוא מיון שעובד תחת ההנחה שטווח המספרים הוא בין $[0,n]$ .
המיון מחזיק מערך מונים כך שהאינדקס ה$i$ במערך המונים מייצג כמה פעמים הופיע המספר $i$ במערך הקלט. לאחר מכן הוא רץ על המערך המונים ופשוט טוען את האיברים למערך חדש לפי הכמות הנתונה לכל איבר _(אם נרצה לקבל מיון יציב, כלומר מיון ששומר על סדר גם בין איברים זהים בערכם, נוכל להוסיף שלב של prefix sum שסוכם את הרישות של כל איבר במערך המונים)_.
סך הכל זמן הריצה הוא $O(n)$.

![Screenshot 2024-02-18 at 21.40.28.png|300](/img/user/Assets/Screenshot%202024-02-18%20at%2021.40.28.png)

ניתן לבצע את שלב המנייה בצורה מקבילית אך יש בכך סיכון של חוסר יציבות באלגוריתם בגלל race condition. במקרה הטוב ביותר בקבוע $O(1)$ נוכל לעדכן את כל המערך במקביל תוך שימוש במנגנוני סינכרוניזציה שונים ובמקרה הגרוע ביותר בשימוש במנגנון סינכרוניזציה נקבל זמן ריצה של $O(n)$ .

ניתן למקבל גם את שלב הצבירה בו מחושבים הסכומים החלקיים של כל האיברים הקודמים. זאת בעית ה Prefix Sum שהיא ניתנת למיקבול. 

אם נרצה לעשות Prefix sum בצורה סדרתית הקוד הוא מאוד פשוט בסיבוכיות ליניארית :

![Pasted image 20240219022723.png|350](/img/user/Assets/Pasted%20image%2020240219022723.png)

בצורה מקבילית יהיה לנו $P_{i}$ לכל איבר $x_{i}$ חוץ מ $x_{0}$ כי אותו לא צריך לסכום. נעשה משהו שמזכיר את הרעיון של horizontal addition ב [[Computer Science/Computer System/SIMD\|SIMD]]. הפסודו קוד יראה כך 

![Pasted image 20240219022706.png|350](/img/user/Assets/Pasted%20image%2020240219022706.png)

הרעיון הוא שככל שמתקרבים לאיברים האחרונים ככה אנחנו מבצעים יותר סכימות ולכן ככל שהלולאה החיצונית מתקדמת ככה התנאי הסכימה נעשת רק על האיברים הימניים יותר במערך. הקפיצה הזאת היא אקספוננציאלית, כלומר עבור $j=2$ כל האיברים באינדקסים $[0,3]$ יחושבו ויתקיים שבמיקומים שלהם יש את הסכום רישות המתאים.

![Screenshot 2024-02-18 at 23.29.47.png](/img/user/Assets/Screenshot%202024-02-18%20at%2023.29.47.png)

![Pasted image 20240218233608.png](/img/user/Assets/Pasted%20image%2020240218233608.png)

זמן הריצה של זה הוא $O(\log n)$ שכן הלולאה הפנימית מתרחשת במקביל.

>[!info] הבחנה
__שלב בניית המערך__ ממויין גם כן ניתן לבצע במקביל על ידי $n$ מעבדים מסונכרנים כדי לבצע בצורה בטוחה פעולת decrement ל counters. זמן ריצה של שלב בניית מערך ממויין יהיה במקרה הטוב $O(1)$ כי באמצעות מערך הסכימה כל תהליך יכול לקבל איבר ולדעת בידיוק מה האינדקס ההתחלתי שלו וגם כמה פעמים צריך לשים אותו במערך החל מאינדקס זה.

>[!warning] נשים לב
>על ידי שימוש בגרסה מקבילית של Prefix Sum ניתן לקבל זמן ריצה של $O(\log n)$ עבור מיון מנייה מקבילי. __אבל__ הגרסה המקבילית לא מחזירה __מיון מציב__ כיוון שלאלגוריתם אין שליטה על התזמון בין ה threads , זה נתון להחלטת מערכת ההפעלה. לכן לפי ההסבר מלמעלה, בשלב בניית המערך אם נסתכל על הדוגמה שלנו, מתקיים ש $P_{2},P_{5},P_{7}$ צריכים שלושתם במקביל לגשת למערך הסכימה, למקם את הערך בתא המתאים ולהחסיר ב$1$ עבור הערך הבא. כשמדובר בתהליכים אין לדעת איזה $P_{i}$ ייגש ראשון למערך הסכימה הזה.


 