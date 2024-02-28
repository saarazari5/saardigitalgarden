---
{"dateCreated":"2023-02-04 19:01","tags":["algorithms","computer_science"],"pageDirection":"rtl","dg-publish":true,"permalink":"/computer-science/algorithms/algorithms-and-matrix-multiplication/","dgPassFrontmatter":true}
---


# כפל מטריצות מהיר
נניח שנרצה להכפיל שתי מטריצות $A,B$ מגודל $n\times n$ ולקבל את מטריצת המכפלה $C$ כאשר

$$c_{ij}= \sum\limits_{k=1}^{n} a_{ik}\cdot b_{kj}$$

__האלגוריתם הנאיבי__ כמובן הוא לחשב את המכפלה לפי הנוסחה לכל $i,j\in[n]$ בזמן ריצה $O(n^{3})$ .

לאורך ההסטורייה הצליחו לחסום את בעיית כפל המטריצות בזמנים נמוכים יותר ויותר . נסמן את החזקה של $n$ שתהווה את החסם לאלגוריתם כפל המטריצות ב $\omega$ . ובאופן כללי נסמן את זמן הריצה של כפל מטריצות ב $O(n^{\omega})$ תחת ההבחנה __שלא יתאפשר__ $\omega<2$ כי חייבים לעבור על המטריצות $A,B$ לפחות פעם אחת לצורך החישוב. 

מספר חסמים שכדאי להכיר שקיימים-
A) נאיבי: $\omega\leq 3$ 
B) [סטרסן](https://en.wikipedia.org/wiki/Strassen_algorithm): $\omega\leq 2.81$ 
C) [קופרסימת ווינוגראד](http://www.cs.utoronto.ca/~yuvalf/Limitations.pdf): $\omega\leq 2.376$ 
D) סתות׳רס: $\omega\leq 2.374$
E) ואסילבסקה וויליאמס: $\omega\leq 2.3729$
F) לה-גאל: $\omega\leq 2.37287$ 

==הגדרת FMM== 
אלגוריתם כפל מטריצות עם זמן $O(n^{3-\varepsilon})$ עבור $\varepsilon>0$ נקרא FMM - כפל מטריצות מהיר. 

==בעיית כפל מטריצות בוליאניות BMM==
_קלט:_ מטריצות בוליאניות $A,B$ מגודל $n\times n$ 
_פלט:_ $C=A\cdot B$ כאשר $c_{ij}=\bigvee\limits_{k=1}^{n} a_{ik}\wedge b_{kj}$   

![Pasted image 20230205004202.png|450](/img/user/Assets/Pasted%20image%2020230205004202.png)

כמובן שאת בעיה זו ניתן לפתור בעזרת FMM בזמן $O(n^{\omega})$ .
__נרצה להראות מספר בעיות שניתן לפתור עם FMM ועם BMM__ 

>[!info] הערה חשובה 
>הרבה מאלגוריתמי FMM הם אינם מעשיים כיוון שהם משתמשים בטכניקות מתמטיות מורכבות כמו הפחתת משוואות לצורכי ייעול הזמן הנאיבי של כפל מטריצות. כאן נכנס לתמונה אלגוריתמים קומבינטוריים שאינם משתמשים בשיטות אלו . המונח אלגוריתם קומבינטורי לא מוגדר היטב ונתייחס אליו כאלגוריתם שלא משתמש בטכניקות הנ״ל . 
>האלגוריתם הקומבינטורי המהיר ביותר כרגע לבעיית BMM רץ בזמן של  $O\left(\frac{n^{3}}{\log^{4}n}\right)$   שלפי סדרי גודל זה ״איטי״ יותר מ $O(n^{2.99999})$ כאשר $n\to\infty$. 
>__השערה__: לא קיים אלגוריתם קומבינטורי שפותר את BMM בזמן $O(n^{3-\epsilon})$ כאשר $\varepsilon>0$ . 
>
אחת הבעיות שנפתור כאן, בעיית זיהוי המשולשים תשתמש ב BMM ונראה שכל ייעול של האלגוריתם הזה ישפר גם את זמן הריצה של BMM בסתירה להשערתנו וכך בעצם קישרנו בין שתי אלגוריתמים שלכאורה הם שונים בתכליתם.

__הגדרה: $\tilde{O}\text{ notation}$__
סימון שמתעלם מכל פקטור פולינומי בזמן הריצה למשל 

$$\displaylines{
O(n^{2}\log^{3}n)=\tilde{O}(n^{2}) \\
O\left(\frac{n^{3}}{\log n}\right) =\tilde{O}(n^{3})
}$$

## זיהוי משולשים בגרף
==הגדרת משולש בגרף==
משולש בגרף לא מכוון הוא מעגל לא מכוון באורך 3 קשתות 

![Pasted image 20230205002753.png|200](/img/user/Assets/Pasted%20image%2020230205002753.png)

==בעיית זיהוי המשולשים== 
_קלט:_ גרף $G=(V,E)$ לא מכוון שמיוצג על ידי [[Computer Science/Algorithms/Graphs basic definitions for CS#מטריצת שכנויות adjacency matrix\|מטריצת שכנויות]] $M$ 
_פלט:_ האם $G$ מכיל משולש או לא.

__הפתרון של הבעיה טמון במשפט הבא:__
עבור המטריצה $M^{3}$ , המחושבת על ידי BMM , האלכסון הראשי ב $M^{3}$ מכיל 1 אמ״מ $G$ מכיל משולש.

__הוכחה:__

$$\displaylines{
	M^{2}=(m_{ij})^{2}= \bigvee\limits_{k=1}^{n} (m_{ik}\wedge m_{kj}) \\
	M^{3}=(m_{ij})^{3}= \bigvee\limits_{r=1}^{n}\bigvee\limits_{k=1}^{n} (m_{ik}\wedge m_{kr})\wedge m_{rj}
}$$

$\leftarrow$ __נניח שהאלכסון הראשי ב $M^{3}$ מכיל $1$, וצריך שקיים משולש ב $G$__ 
אם כן קיים $i$ כלשהו כך ש $m_{ii}^{3}=1$ כלומר 

$$\bigvee\limits_{r=1}^{n}\bigvee\limits_{k=1}^{n} (m_{ik}\wedge m_{kr})\wedge m_{ri}=1$$

כלומר קיים $r$  עבורו 

$$\bigvee\limits_{k=1}^{n} (m_{ik}\wedge m_{kr})\wedge m_{ri} =1$$

ולכן כל אחד מהגורמים הנ״ל הוא $1$ , $m_{ri}=1$ וגם $\bigvee\limits_{k=1}^{n} (m_{ik}\wedge m_{kr})=1$ ולכן קיים $k$ כשלהו עבורו 

$$m_{ik}\wedge m_{kr}=1 \leftrightarrow m_{ik}=1  \text{ and } m_{kr}=1$$

התוצאה היא שקיבלנו 3 קשתות $m_{ik}=m_{kr}=m_{ri}=1$ כלומר הקשתות $(i,k),(k,r),(r,i)$ שזה בעצם משולש.

$\rightarrow$ __נניח שב $G$ יש משולש__ 
נסמנו $(a,b),(b,c),(c,a)$ . המשמעות היא שב $M$ מתקיים

$$m_{ab}=m_{bc}=m_{ca}=1$$

נסמן : $i=a,k=b,r=c$ ומהנוסחה שפתחנו עבור ההוכחה הקודמת יתקיים $m_{ii}=m_{aa}=1$ .

> [!info] הוכחה כללית יותר
> נוכל להוכיח את הטענה באופן כללי יותר - תהי מטריצת $M$ מטריצת שכנויות עבור $G=(V,E)$ 
> אזי, במטריצה $M^{n}$ המחושבת על ידי BMM יתקיים שב $(m_{ij})^{n}=1$  אמ״מ קיים מסלול מ $i$ ל $j$ באורך $n$
> רעיון ההוכחה יהיה להראות על $M^{2}$ את הדרוש (נובע ישירות מהגדרת הכפל הבוליאני) ולהתייחס לזה ואל $M$ עצמה כבסיס האינדוקצייה (שכן מטריצת שכנויות מראה את כל המסלולים באורך אחד מ $i$ ל $j$) , לאחר מכן כצעד האינדוקצייה ניקח את $M^{n}$ ויתקיים $M^{n}=M^{n-1}\cdot M$ כלומר לפי כפל מטריצות בוליאני והנחת האינדוקצייה , יתקיים שאנחנו בודקים האם ישנו קודקוד $k$ כלשהו שמחבר בין $i,j$ הדרושים כאשר נתייחס ל $i$ כאינדקס המייצג את השורה במטריצה $M^{n-1}$ כלומר לקחנו מסלול מאורך $n- 1$ בין $i$ ל $k$ וסך הכל בודקים אם ניתן לחבר את ה $k$ הזה לקודקוד $j$ . 
> __כמובן שאם מתקיים $(m_{ii})^{n}=1$ משמעות הדבר היא שיש מעגל באורך $n$ לקודקוד $i$__
> 
> __באופן דומה__ נוכל להוכיח שאם נעלה מטריצה $M$ בחזקת $n$ בכפל רגיל נקבל את מספר המסלולים באורך $n$ מ קודקוד $i$ ל $j$ עבור איבר $(m_{ij})^{n}$ 


==משפט:==
אם ניתן לפתור את בעיית זיהוי המשולשים קומבינטורית בזמן $\tilde{O}(V^{3-\varepsilon})$ אז ניתן לפתור את בעיית $BMM$ בזמן $\tilde{O}(n^{3-\frac{\varepsilon}{3}})$ .

זה הקשר שהזכרתי בתחילת הסיכום שאמרתי שנסתכל עליו שתמיד יעמוד בסתירה להנחה שלא ניתן לפתור את בעיית BMM בזמן $O(n^{3-\varepsilon})$ .

__הוכחה__: 
קודם כל , נראה דרך שבהינתן אלגוריתם קומבינטורי שפותר בעיית זיהוי משולשים בגרף , אז ניתן לנצל את זה ולבנות אלגוריתם שפותר BMM.
אם כן בהינתן שתי מטריצות $A,B$ מגודל $n\times n$ נרצה לחשב את בעיית המכפלה $C=AB$ כאשר הכפל בוליאני. 
לאחר מכן, ננתח זמן ריצה ונראה שבעיית זיהוי המשולשים משפיעה על זמן הריצה של גרף זה.

נבנה גרף תלת צדדי $G=(V,E)$ (כלומר גרף שהקשתות הן רק בין הקבוצות שנגדיר כמו ב [[Computer Science/Discrete Math/Graph theory basics#גרף דו-צדדי\|גרף דו צדדי]])  כאשר 

$$\displaylines{
V=I\cup J\cup K \\
I=(i_{1},\dots,i_{n})\\
J=(j_{1},\dots, j_{n})\\
K=(k_{1},\dots,k_{n})
}$$

נגדיר את הקשתות באופן הבא 

$$E=\{(i_{\alpha},j_{\beta})\ | \ a_{\alpha\beta}=1\}\cup\{(j_{\alpha},k_{\beta})\ | \ b_{\alpha\beta}=1\}\cup \{(k_{\alpha},i_{\beta})\ | \ k_{\alpha}\in K , i_{\beta}\in I \}$$

__במילים__ איפה שיש $1$ במטריצה $A$ נבנה קשת בין $I$ ל $J$ ואיפה שיש $1$ ב $B$ יש קשת בין $J$ ל $K$ ונוסיף את כל הקשתות האפשריות בין $K$ ל  $I$ . _נשים לב שיש לנו $3n$ קודקודים ו $O(n^{2})$ קשתות_.

לדוגמה- עבור המטריצות

$$A=\begin{pmatrix}0&1 \\ 1&0 \end{pmatrix} \ \ B=\begin{pmatrix} 0&0 \\ 1& 1 \end{pmatrix} \ \ C=AB=\begin{pmatrix}1&1\\0&0\end{pmatrix}$$

הגרף שיווצר יהיה

![Screenshot 2023-02-05 at 2.01.28.png|400](/img/user/Assets/Screenshot%202023-02-05%20at%202.01.28.png)
_לשם חלק מהוכחות נשתמש בצבעים האלה תמיד בקונבצייה לייצוג קשת מסויימת בין צדדים. למשל מ $I$ ל $J$ זה תמיד יהיה כחול_.

כעת נוכיח טענת עזר ומשם ניגש לניסיונות שלנו להוכיח את הטענה הנ״ל-

==למה== 
עבור $k_{\gamma}\in K$ ו $i_{\alpha} \in I$  הקשת $(i_{\alpha},k_{\gamma})\in E$ משתתפת במשולש אמ״מ $c_{\alpha\gamma}=1$ .

__הוכחה:__
משולש הוא קשת אדומה, כחולה, ירוקה.
ניקח קשת אדומה כלשהי $(i_{\alpha} , k_{\gamma})$ (כי אלו מתקיימות תמיד מהגדרת קשתות אלו), לכן שאר הקשתות במשולש צריכות להיות ירוקה אחת וכחולה אחת.
הכחולה אומרת שתא ב $A$ שווה ל $1$ נניח שהתא הוא $a_{\alpha\beta}$ ובגרף זאת הקשת $(i_{\alpha},j_{\beta})$ ובאופן דומה, הירוקה אומרת שתא ב $B$ הוא $1$ . אם התא הזה הוא $b_{\beta\gamma}$ בגרף זאת תהיה הקשת $(j_{\beta},k_{\gamma})$  ויש לנו משולש. כמו כן כפי שמוגדר כפל מטריצות בוליאני אז 

$$c_{\alpha\gamma}= \bigvee\limits_{r=1}^{n} a_{\alpha r}\wedge b_{r\gamma}= (a_{\alpha1}\wedge b_{1\gamma})\vee(a_{\alpha2}\wedge b_{2\gamma})\vee\dots\vee \overbrace{(a_{\alpha\beta}\wedge b_{\beta\gamma})}^{1}\vee\dots\vee(a_{\alpha n}b_{n\gamma})=1$$
לכן הוכחנו את שתי הצדדים, אם יש משולש בגרף בהכרח ערך המתאים ב $C$ יהיה $1$.

>[!tip] קונבנצייה
>קל יותר לרשום משולש בגרף הנ״ל  כ $(i_{\alpha},j_{\beta},k_{\gamma})\in I\times J\times K$ במקום $(i_{\alpha},b_{\beta},k_{\gamma},i_{\alpha})\in I\times J\times K\times I$ כיוון שנתון מהגדרה שתמיד יש קשת בין $K$ ל $I$.

==ניסיון 1== 
נראה ראשית, איך מחשבים את מטריצת המכפלה דרך שימוש באלגוריתם זיהוי משולשים בגרף.

``` psuedo
	BMM(A,B)
		create  G = (IxJxK, E) from A,B
		C = new matrix[n,n] = {0,0}
		while G contains triangle (i_t,j_l,k_p)
			C[t,p] = 1
			delete (i_t, k_p) from G
```
_מחיקת הקשת מבטיחה שתמיד נשבור משולש שהיה בגרף וככה לא נחזור על משולשים פעמיים והאלגוריתם חייב להגמר מתישהו_.

__הוכחת נכונות__:
נשווה את $c_{\alpha,\gamma}$ מתוצאת הכפל הנאיבי ונראה שהאלגוריתם אכן ימקם את הערך המתאים לאיבר זה.
נניח שהנ״ל ערכו $1$ מהלמה שהוכחנו אנחנו יודעים ש $(i_{\alpha},k_{\gamma})\in E$ משתתפת במשולש , כיוון שהשתמשו בזיהוי משולשים כקופסה שחורה ומניחים את נכונותו הרי שהאלגוריתם יזהה אותו ויציב $c_{\alpha,\gamma}=1$ וימחק את הקשת הזאת.

אם $c_{\alpha\gamma}=0$ אז מהלמה הנ״ל הקשת $(i_{\alpha},k_{\gamma})\in E$ לא שייכת למשולש  , האלגוריתם לא יזהה את הקשת הזאת כחלק מהמשולש ולא ישנה את ערכה.

>[!warning] בעיות מהאלגוריתם
>ישנן שתי בעיות עיקריות שעולות מהאלגוריתם הנ״ל
>הראשונה, היא העובדה שאנחנו תיארנו אלגוריתם שמחזיר true אם יש משולש ו false אם לא. לא תיארנו אלגוריתם שמחזיר את הקשת עצמה כדי שנוכל להשתמש בה כמו שעשינו. ניתן באופן נאיבי לבנות אלגוריתם בזמן $O(|V|^{3})$ שמחזיר משולש במידה והוא מוצא כזה. 
>
>השנייה, היא זמן הריצה , אנחנו מבצעים $O(n^{2})$ איטרציות לזיהוי משולשים בגרף זאת כיוון שמקרה קצה יש משולש לכל $(i_{\alpha},k_{\gamma})$ וכיוון שכל $I$ משוייך לכל $K$ בגרף יש $n^{2}$ קשתות כאלו. סך הכל יתקיים שזמן הריצה הכולל של האלגוריתם עבור הגרף שלנו עם $3n$ קודקודים יהיה
>
>$$\tilde{O}(n^{2}\cdot |V|^{3-\varepsilon})=\tilde{O}(n^{2}\cdot 3n^{3-\varepsilon})= O(n^{5-\varepsilon})$$
>
>שזה זמן גדול בהרבה מהזמן הנאיבי ולכן לא הראנו כלום.

==ניסיון 2== 
הרעיון יהיה לבצע מעין [[Computer Science/Algorithms/Divide and conquer\|הפרד ומשול]] של הגרף. 
נחלק את $I,J,K$ ל $t$ תתי חלוקות כשבכל תת חלוקה $\frac{n}{t}$ קודקודים

$$\displaylines{
I= \bigcup_{i=1}^{t} I_{i}\\ J= \bigcup_{i=1}^{t} J_{i} 
\\ K= \bigcup_{i=1}^{t} K_{i}
}$$
נשים לב שיש לנו $t^{3}$ שלישיות אפשריות $(I_{x},J_{y},K_{z})$ ועליהן נריץ את האלגוריתם מהניסיון הראשון.

```psuedo
BMM(A,B)
	create  G = (IxJxK, E) from A,B
	C = new matrix[n,n] = {0,0}
	divide I,J,K to t sub sets. (each sub group will have n/t vertex)

	for each (I_x, J_y, K_z)
		while sub graph of those vertex containg edges (i_t, j_l, k_p)
			C[t,p] = 1
			delete (i_t,k_p) from G

```

__הוכחה נכונות:__
לפני שבכלל נבדוק מיהו $t$ שיאפשר זמן ריצה אופטימלי. נוכיח שאכן התוצאה בסוף של $C$ נכונה ותהיה שקולה לכפל המטריצות הנאיבי.
בדומה להוכחה הקודמת אם ניקח שלישייה $(i_{\alpha},j_{b},k_{\gamma})$ משלישייה $(I_{x},J_{y},K_{z})$ כך שהיא מייצרת משולש בגרף $G$  אז אנחנו יודעים בוודאות שהקשת $(i_{\alpha},k_{\gamma})$ לא נמחקה והיא כעת תמחק באיטרצייה של השלישייה הנ״ל לאחר שתבוצע ההשמה $c_{\alpha\gamma}=1$ . לאחר מכן הקשת תמחק ולא נוכל יותר לזהות משולש עם הקשת $(i_{\alpha},k_{\gamma})$ גם אם נרוץ על שלישייה שמכילה את $I_{x},K_{z}$ בתוכה.

__זמן ריצה:__
נרצה לספור כמה פעמים האלגוריתם קורא לזיהוי משולשים בגרף מגודל $\frac{3n}{t}$ (גודל של שלישייה אחת).
* מספר ההשמות במטריצה $C$ הוא לכל היותר $n^{2}$ כגודל המטריצה.
* מספר הפעמים שבהם לא נזהה משולש הוא לכל היותר $t^{3}$ כיוון שכל פעם שלא מוצאים משולש מיד עוברים לשלשה חדשה.

סך הכל מספר הפעמים שמפעילים את האלגוריתם לזיהוי משולשים הוא $t^{2}+n^{3}$ .
נסמן ב $T(n)$ להיות זמן הריצה לאלגוריתם זיהוי משולשים על גרף עם $n$ קודקודים וזמן הריצה של כל האלגוריתם שלנו יהיה:

$$(t^{3}+n^{2})T\left( \frac{3n}{t}\right)$$
כלומר עבור $t= n^\frac{2}{3}$ נקבל זמן ריצה 

$$n^{2}T(3\cdot n^{\frac{1}{3}})=n^{2}O\left(\left(n^{\frac{1}{3}}\right)^{3-\varepsilon}\right)= O(n^{3-\varepsilon})$$

## APSP על גרף לא מכוון ולא משוקלל
אנחנו מכירים דרכים לפתרון בעיית [[Computer Science/Algorithms/All-Pairs Shortest Paths (APSP)\|APSP]] על גרפים מכוונים ומשוקללים וכמובן שניתן לפתור באמצעותם את הבעייה הנ״ל , שכן היא מקרה פרטי של המקרים שכבר פתרנו. אבל אם נתון לנו שהגרף לא מכוון ולא משוקלל נוכל לפתור את הבעיה במספר דרכים יעילות בינהן __אלגוריתם של סיידל__ שמאפשר לפתור את הבעייה באמצעות כפל מטריצות מהיר.

>[!info] הבחנה
>כפי שכבר הראנו, ניתן באמצעות [[Computer Science/Algorithms/BFS\|BFS]] לפתור את בעיית הAPSP עבור גרף מכוון או לא מכוון כל עוד אין פונקציית משקלים. ולכן אם אנחנו יודעים שהגרף לא ממושקל אבל לא יודעים אם הוא מכוון או לא מכוון נוכל להריץ BFS מכל קודקוד מקור ולקבל זמן ריצה $O(V^{2}+V\cdot E)$ 
>

__הגדרה:__
_קלט-_ גרף $G=(V,E)$ קשיר, לא מכוון , לא ממושקל וללא קשתות כפולות שמיוצג על ידי מטריצת שכנויות $A$ .
_פלט-_ לכל $u,v\in V$ נחשב את המסלול הקצר ביותר (במספר הקשתות) בין $u$ ל $v$.

==נשים לב-== שהגדרנו שהגרף קשיר בלי הגבלת הכלליות, אחרת פשוט נריץ את אותו אלגוריתם על כל רכיב קשירות בנפרד, בנוסף במטריצת השכנויות במקרה הזה האלכסון הראשי מכיל רק אפסים כי אין קשתות עצמיות.

כדי להתחיל לבנות את האלגוריתם, נגדיר גרף חדש 

$$G^{\prime}=(V,E^{\prime})$$

המיוצג על ידי מטריצת השכנויות 

$$A^{\prime}= A^{2}\vee A$$

המשמעות של $A^{\prime}$ היא ש $a^{\prime}_{ij}=1\leftrightarrow{(a^{2}_{ij}=1)\vee a_{ij}=1 }$  כלומר, במטריצת יהיה $1$ במיקום $ij$ אם ורק אם יש מסלול באורך 2 מ קודקוד $i$ לקודקוד $j$ __או__ שהקודקוד ה $i$ והקודקוד ה $j$ הם שכנים. זה נובע מההבחנה שעשינו בתחילת הסיכום ש $A^{n}$ מייצג את התכנות המסלולים באורך $n$ בין שתי קודקודים $i,j$ בגרף.

__בגלל שהגרף המקורי $G$ הוא קשיר ולא מכוון, הרי שב $A^{2}$ יהיו איברים באלכסון שערכם יהיה $1$ כיוון שאם יש מסלול באורך 2 מ $i$ ל $j$ בגרף אז בהכרח גם יש מסלול באורך $2$ מ $j$ ל $i$. תחת ההבחנה הזאת נשים לב שבגלל שבגרף שמיוצג על ידי $A^{\prime}$ אסור שיהיו לולאות עצמיות, נדאג לאפס את איברי האלכסון שלו__.

הקשתות של הגרף יהיו 

$$E^{\prime}= E\cup \{(u,v) \ | \ \exists _{w\in V}: (u,w),(w,v)\in E\}$$

כלומר בנוסף לכל הקשתות שיש ל $G$ , הגרף $G^{\prime}$ יבנה קשת נוספת בין כל הקודקודים שבגרף $G$ היה בהם מסלול מאורך 2.

![Pasted image 20230205144929.png|300](/img/user/Assets/Pasted%20image%2020230205144929.png)

==טענה== אורך המסלול הקצר ביותר בין $u,v\in V$ בגרף $G^{\prime}$ שקול לחצי מעוגל כלפי מעלה של המסלול הקצר ביותר בינהם בגרף $G$. 

$$\delta^{\prime}(u,v)=\left\lceil\frac{\delta(u,v)}{2}\right\rceil $$
__הוכחה__:
נסמן $p$ את המסלול הקצר ביותר מ $u$ ל $v$ בגרף $G$
נחלק למקרים- 
א) $\delta(u,v)= 2k$ כלומר $p=(v_{1},v_{2},v_{3}\dots,v_{2k+1})$ . מההגדרה, ב $G^{\prime}$ יש את המסלול $p^{\prime}=(v_{1},v_{3},\dots,v_{2k-1},v_{2k+1})$ כלומר מסלול באורך $k$ קשתות. 
אם כן , נניח בשלילה ש מסלול זה אינו הקצר ביותר ב $G^{\prime}$ מ $u$ ל $v$ כלומר קיים מסלול אחר $p^{*}$ שהוא מינימלי כלומר $|p^{*}|< k$ . מהגדרת הקשתות בגרף $G^{\prime}$ אנחנו יודעים שב $G$ יש מסלול שמתאים ל $p^{*}$ עם מספר קשתות שקול או לכל היותר $2|p^{*}|<2k$ בסתירה למינימליות של $p$ . 
כלומר $p^{\prime}$ הוא מסלול קצר ביותר ב $G^{\prime}$ מ $u$ ל $v$ ויתקיים 

$$\delta^{\prime}(u,v)=\left\lceil\frac{\delta(u,v)}{2}\right\rceil = \frac{\delta(u,v)}{2}$$

ב) $\delta(u,v) = 2k+1$ כלומר $p=(v_{1},v_{2},v_{3}\dots,v_{2k+2})$ לפי הגדרת $G^{\prime}$ קיים מסלול $p^{\prime}$ בגרף זה כל ש 
$p^{\prime}=(v_{1},v_{3}\dots,v_{2k-1},v_{2k+1},v_{2k+2})$ כלומר מסלול עם $k+1$ קשתות. נניח בשלילה שהוא אינו המסלול הקצר ביותר וקיים $p^{*}$ קצר יותר. כלומר $|p^{*}|<k+1$ ולכן יש ב $G$ מסלול מתאים ב $p^{*}$ עם לכל היותר $2|p^{*}|<2k+1$ בסתירה למינימליות של $p$ . כלומר 

$$\delta^{\prime}(u,v)= k+1 = \frac{\delta(u,v)-1}{2}+1 = \left\lceil\frac{\delta(u,v)}{2}\right\rceil$$

האלגוריתם הבא פותר את בעיית APSP לגרפים לא מכוונים ולא ממושקלים.
נסמן $\Delta$ כמטריצת המרחקים הסופית של $G$ ובאופן שקול $\Delta^{\prime}$ כמטריצת המרחקים הסופים של $G^{\prime}$ 

``` psuedo
seidel_generic(A):
	if A is all 1 except for the diagonal 
		return A
	𝚫' = seidel_generic(A*A V A)
		for u,v in V
			if 𝚫[u,v] is odd
				𝚫[u,v] = 𝜹(u,v) = 2𝜹'(u,v)-1
			else 
				𝚫[u,v] = 𝜹(u,v) = 2𝜹'(u,v)
		return 𝚫
```

נסביר, האלגוריתם בונה כל פעם מטריצה חדשה עד אשר תנאי הבסיס שבו יש קשת ישירה מכל קודקוד בגרף $G^{\prime}$ מתקיים (כיוון שהגרף קשיר אנחנו נבצע את הפעולה הזאת לכל היותר $\log |V|$ פעמים) ולאחר מכן יורדים בחזרה כאשר אנחנו מפרקים אחורנית את ערך המסלול הקצר ביותר לפי המשפט שהראנו למעלה. 

__נשים לב שבדקנו האם המרחק הקצר ביותר $\delta(u,v)$ בין שתי קודקודים הוא זוגי או אי זוגי מבלי לחשב אותו כלל, איך עשינו זאת?__

יהי $u,v\in V$ , נניח ש $w\in V$ שכן של $v$ . על פי [[Computer Science/Algorithms/Single-Source Shortest Paths (SSSP)#תכונות של relaxation\|למת אי שיוויון המשולש ]] :

$$\displaylines{
\delta(u,v)\leq \delta(u,w)+1 \\
\delta(u,w)\leq \delta(u,v)+1 \\ \downarrow \\
\delta(u,v)-1 \leq \delta(u,w)\leq \delta(u,v)+1
}$$

יש 4 מקרים עבור הזוגיות של $\delta(u,v)$ ו $\delta(u,w)$
__א)__ $\delta(u,v)\equiv_{2}\delta(u,w)$ כלומר הם שקולים מודולו 2. המשמעות של זה היא ששניהם זוגיים או אי זוגיים ביחד ובהכרח יתקיים שיוויון בינהם (כיסינו 2 מקרים).
הסיבה היא- 
שאם שניהם זוגיים נסמן $\delta(u,v)=2k$ ו  $\delta(u,w)=2r$ ולפי מה שרשמנו למעלה

$$\displaylines{
2k-1\leq 2r\leq 2k+1 
}$$

המספר השלם היחיד שנמצא בין $2k-1$ ל $2k+1$ הוא $2k$ ולכן $2k=2r$ ולכן המרחקים שווים.

באופן דומה אם שניהם אי זוגיים נקבל ש 

$$2k\leq 2r+1\leq 2k+2$$

וההסבר שקול למה שעשינו במקרה הזוגי $2k+1$ הוא המספר השלם היחיד שנמצא בין שתי מספרים אלו.

![Pasted image 20230205164258.png|250](/img/user/Assets/Pasted%20image%2020230205164258.png)
נשים לב שהמשמעות של תרחיש זה היא ש $w$ אינו במסלול הקצר ביותר מ $u$ ל $v$ ובאופן דומה $v$ אינו במסלול הקצר ביותר מ $u$ ל $w$. זה נובע מאי שיוויון המשולש .

__ב)__ אם $\delta(u,v)$ זוגי ו $\delta(u,w)$ אי זוגי:

$$\delta^{\prime}(u,w)= \left\lceil \frac{\delta(u,w)}{2}\right\rceil= \frac{\delta(u,w)+1}{2}\geq \frac{\delta(u,v)-1+1}{2}= \frac{\delta(u,v)}{2}=\delta^{\prime}(u,v)$$
כלומר 

$$\delta^{\prime}(u,w)\geq \delta^{\prime}(u,v)$$

__ג)__  אם $\delta(u,v)$ אי זוגי ו $\delta(u,w)$ זוגי:

$$\delta^{\prime}(u,w)= \left\lceil \frac{\delta(u,w)}{2}\right\rceil= \frac{\delta(u,w)}{2}\leq \frac{\delta(u,v)+1}{2}= \left\lceil\frac{\delta(u,v)}{2}\right\rceil=\delta^{\prime}(u,v)$$

כלומר 

$$\delta^{\prime}(u,w)\leq \delta^{\prime}(u,v)$$

>[!info] הבחנה
>אם $x$ הוא שכן של $v$ במסלול הקצר ביותר וגם $\delta(u,v)$ הוא אי זוגי.
> אז האי שיוויון הוא ממש כלומר: 
> $\delta^{\prime}(u,x)< \delta^{\prime}(u,v)$ 
> הסיבה שזה נכון היא שאנחנו יודעים ש $\delta(u,x)=\delta(u,v)-1$  ובגלל ש $\delta(u,v)$ אי זוגי אז $\delta(u,x)$ זוגי ולכן
> 
> $$\delta^{\prime}(u,x)= \frac{\delta(u,x)}{2}= \frac{\delta(u,v)-1}{2}< \frac{\delta(u,v)+1}{2}=\delta^{\prime}(u,v)$$ 

__הגדרה:__ נסמן ב $N(v)$ את הקודקודים השכנים של $v$ ב $G$ . נשים לב- 

$$|N(v)|=\deg(v)$$


==טענה== 

$$\delta(u,v) \text{ is even } \leftrightarrow \sum\limits_{w\in N(v)}\delta^{\prime}(u,w)\geq \deg(v)\cdot \delta^{\prime}(u,v)$$

__הוכחה__
$\leftarrow$ : $\delta(u,v)$ זוגי ולכן לכל $w$ שכן של $v$ יתקיים $\delta(u,w)$ זוגי או אי זוגי כלומר ממה שהוכחנו 

$$\sum\limits_{w\in N(v)}\delta^{\prime}(u,w)\geq \sum\limits_{w\in N(v)}\delta^{\prime}(u,v)=\deg(v)\cdot \delta^{\prime}(u,v)$$

$\rightarrow$ נוכיח לפי הcontra positive של הטענה. נניח $\delta(u,v)$ אי זוגי כלומר ממה שהוכחנו למעלה אנחנו יודעים שהמקרים שמתאפשרים על שכן $w$ הוא ש $\delta^{\prime}(u,w)\leq \delta^{\prime}(u,v)$ כלומר 

$$\sum\limits_{w\in N(v)}\delta^{\prime}(u,w)\leq \sum\limits_{w\in N(v)}\delta^{\prime}(u,v) \leq \deg(v)\cdot \delta^{\prime}(u,v)$$

רק נשאר להסביר למה האי שיוויון הזה הוא חזק כלומר במקום $\leq$ צריך להיות $<$ הסיבה לכך היא שכפי שאמרנו בהבחנה שלנו עבור השכן $w$ שהוא חלק מהמסלול הקצר ביותר יתקיים אי שיוויון חלש   כלומר כל הסכום הזה יהיה קטן ממש ולא קטן או שווה ולכן האי שיוויון חלש וזה מוכיח את הcp של הטענה שלנו. 

__אם כן אנחנו יכולים לקבוע את הזוגיות של מסלול קצר ביותר מבלי לחשב אותו ישירות בעזרת ערכי $\delta^{\prime}$ שלו . מטרתנו כעת היא להבין איך מחשבים את הגורמים הרלוונטים לקביעת הזוגיות באופן יעיל.__ 

א) על מנת לחשב את דרגת הקודקוד עלינו לסרוק את שורת המטריצה של $v$ כלומר יעלה לנו $O(V)$ זמן.
אם כן, חישוב הדרגה לכל הקודקודים הוא $O(V^{2})$. סך הכל בהינתן $\delta^{\prime}$ נוכל לחשב את הביטוי $\deg(v)\cdot \delta^{\prime}(u,v)$ בזמן  $O(V^{2})$ לכל קודקודי הגרף.

ב) כדי לחשב את $\sum\limits_{w\in N(v)}\delta^{\prime}(u,w)$ נגדיר את המטריצה $M=\Delta^{\prime}\cdot A$  (הכפל הוא רגיל ולא בוליאני). נוכיח ש

$$m_{u,v}= \sum\limits_{w\in N(v)}\delta^{\prime}(u,w)$$

__הוכחה__ - 

$$\displaylines{
m_{uv}= (\Delta^{\prime}\cdot A)_{uv}= \sum\limits_{w=1}^{n}\delta^{\prime}(u,w)\cdot a_{w,v}= \sum\limits_{w=1}^{n}\delta^{\prime}(u,w)\cdot \begin{cases}
1&(w,v)\in E\\0&else 
\end{cases}
\\ 
\sum\limits_{w=1}^{n}\delta^{\prime}(u,w)\cdot \begin{cases}
1&w\in N(v)\\0&else 
\end{cases} = \sum\limits_{w\in N(v)} \delta^{\prime}(u,w)
}$$

## האלגוריתם של סיידל
אם כן מצאנו דרך לחשב את שתי הביטויים הדרושים כדי לקבוע זוגיות של מסלול קצר ביותר מבלי לחשב את ערכו. סך הכל האלגוריתם של סיידל ייראה כך

```psuedo
seidel(A):
	if A is all 1 except for the diagonal
		return A
	𝚫' = seidel(A*A V A)
	M = 𝚫'* A
	for u,v in V
		if M[u,v] < deg(v)𝚫'[u,v]
			𝚫[u,v] = 2𝚫'[u,v]-1
		else
			𝚫[u,v] = 2𝚫'[u,v]
	return 𝚫
```

__זמן ריצה__ 
אם אורך המסלול הארוך ביותר בגרף הוא $d$ , אז צריך $\Theta(\log d)$ איטרציות לפני שנגיע למקרה הבסיס במקרה הגרוע $d=|V|$ .  בנוסף לכל יש את העבודה המקומית שהיא ביצוע כפל מטריצות מהיר FMM וחישוב דרגות כל קודקודי הגרף. זמן העבודה המקומית הוא 

$$O(V^{2}+ V^{\omega})= O(V^{\omega})$$
כי כפי שאמרנו $\omega>2$ בהכרח. ביחד עם הקריאה הריקורסיבית והתייחסות לעומק עץ הריקורסייה זמן הריצה יהיה

$$O(V^{\omega}\log(|V|))$$

>[!note] גרף קליק
>הגדרה לתנאי העצירה שלנו נקראת גרף קליק, כלומר גרף מכוון שמכיל את כל הקשתות לדוגמה
>
> ![Pasted image 20230205180854.png|250](/img/user/Assets/Pasted%20image%2020230205180854.png)
> 
> במצב זה, מטריצת השכנויות תהיה מטריצה שכל האיברים הם $1$ פרט לאיברי האלכסון שכן אין קשתות עצמיות (זה אינטואיטיבית הגיוני כי כולם שכנים של כולם).

