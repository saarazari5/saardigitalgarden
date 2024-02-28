---
{"dateCreated":"2022-12-27 23:48","tags":["algorithms","computer_science"],"pageDirection":"rtl","dg-publish":true,"permalink":"/computer-science/algorithms/single-source-shortest-paths-sssp/","dgPassFrontmatter":true}
---



# Single-Source Shortest Paths (SSSP)

## מסלולים קצרים ביותר בגרף
אחת הבעיות הנפוצות ביותר בגרפים היא מציאת המסלול הקצר ביותר. אך לבעיה זאת יש מספר סוגים והיא משתנה בהתאם להאם הגרף מכוון או לא והאם הוא ממשוקל או לא.
באופן מסויים,  אם הקלט הוא גרף לא מכוון נוכל תמיד להפוך כל קשת לשתי קשתות שמכוונות כל אחת לכיוון השני ואם הוא לא ממושקל נוכל תמיד להגדיר משקל של $1$ על כל קשת כלומר __העלות__ של מסלול מסויים היא אורכו __והעלות__ של מסלול בגרף ממושקל היא סכום המשקלים של כל קשת במסלול.
בכל מקרה נסמן 

$$\delta(u,v)=\begin{cases} \min(w(p) \ :  u\overset{p}{\rightarrow} v\ ) & \text{if there is a path between u and v} \\ \infty & otherwise \end{cases}$$

כעלות המינימלית ביותר של מסלול מ $u$ ל $v$ .

ישנן מספר וריאציות לבעית המסלול הקצר ביותר 
א) single pair- בהינתן גרף $G$ ו שתי קודקודים $u,v$ נרצה למצוק את המסלול הקצר ביותר (בעל העלות הנמוכה ביותר) מ $u$ ל $v$.
ב) single source- זאת הבעיה בה נתמקד כאן, SSSP , בהינתן גרף $G$ וקודקוד מקור $s$ נרצה לחשב עבור כל הקודקודים האחרים ב $V$ את המסלול הקצר ביותר.
ג) all pairs- זאת בעיית [[Computer Science/Algorithms/All-Pairs Shortest Paths (APSP)\|All-Pairs Shortest Paths (APSP)]],  מחשב את המסלול הקצר ביותר לכל זוג קודקודים $u,v$ בגרף.

נשים לב שאלגוריתם ל APSP הוא גם אלגוריתם תקף ל SSSP והוא אלגוריתם תקף ל SPSP. כמו כן אין אלגוריתם ידוע עבור SPSP (single pair) שהוא יותר יעיל מפשוט לפתור את SSSP .

## הגדרת SSSP
אם כן ישנם שתי מקרים שבהם נוכל להתמקדם , הראשון הוא גרף לא ממושקל, במצב זה נוכל לפתור את הבעיה באמצעות [[Computer Science/Algorithms/BFS\|BFS]] .כאן, אתמקד בגרף המכוון שכן האלגוריתם שיתואר כאן כפי שאמרנו, מתאים גם עבור גרף לא מכוון פשוט פחות יעיל מהפתרון לעיל.

## עץ מסלולים קצרים ביותר
ב  SSSP, ישנם $|V|$ מסלולים שעלינו להחזיר מה שאומר שרק כדי לשמור את כל המידע נצטך $\Theta(V^{2})$ זמן. נוכל להשתמש בתכונה הבאה כדי לייעל את התהליך:

__תתי המסלולים של מסלול קצר ביותר, קצרים ביותר__- בהינתן $G=(V,E)$ גרף מכוון, ממושקל עם $w: E\to\mathbb{R}$ . ויהי $P=(v_{0},\dots, v_{k})$ המסלול הקצר ביותר מ $v_{0}$ ל $v_{k}$ , אזי עבור כל $0\leq i\leq j\leq k$ יתקיים שתת המסלול $P_{i,j}$ של $P$ מ $v_{i}$ ל $v_{j}$ הוא מסלול קצר ביותר בינהם.

_הוכחה_: נחלק את P כך-

$$P= v_{0}\to v_{i}\to v_{j}\to v_{k}$$

ואז מתקיים 

$$w(P)= w(P_{0,i})+w(P_{i,j})+ w(P_{j,k})$$

אם היה קיים $P^{*}_{i,j}$ שמשקלו קטן מהמשקל $w(P_{i,j})$ אז היינו להחליף את $P_{i,j}$ במסלול זה ולקבל $P^{*}$ עם משקל מינימלי _קטן ממש_ ממשקל $P$ . בסתירה לכך ש $P$ הוא מסלול עם עלות נמוכה ביותר.

אם כן נקבל את ההבחנה הבאה ממשפט זה:
המסלול בעל עלות נמוכה ביותר $s\to u$ מכיל גם את המסלולים הקצרים ביותר מ $s$ לכל הקודקודים האחרים ששוכנים על המסלול הזה. כלומר נוכל להסתכל על $s$ כשורש של עץ שילדיו הם המסלולים הכי קצרים מ $s$ לשכניו וכן הלאה עד שמגיעים לכל הקודקודים.
באופן כללי המטרה שלנו היא לבנות את העץ הזה בחישוב SSSP. באופן הזה נוכל להמנע מלחשב $|V|$ מסלולים קצרים ביותר שכן יש פה מעין אלגוריתם חמדני שמאפשר לנו לבנות את המסלול הקצר ביותר מתת המסלול הקצר ביותר.
כמו כן , נשים לב ש APSP ניתן לחישוב על ידי בנייה של עץ כזה כאשר כל פעם קודקוד אחר הוא השורש. סך הכל $|V|$ עצי מסלולים קצרים ביותר.

## קשתות שליליות
בבעיית SSSP ייתכן וגרף הקלט מכיל קשתות עם משקל שלילי: 
א)  אם הגרף __לא מכיל מעגלים שליליים__ (מסלול שהוא מעגל שסכום הקשתות שלו שלילי) מקדוקוד המקור $s$ אז לכל $v\in V$ , משקל המסלול המינימלי $\delta(s,v)$ _מוגדר היטב_ גם אם ערכו שלילי. 
ב)  אם הגרף מכיל מעגל שלילי מ $s$ , משקל המסלול לא מוגדר היטב שכן תמיד נוכל להמשיך לנוע על המעגל השלילי עד $-\infty$ וזה יהיה המסלול בעל עלות מינימלית.

יש כמה אלגוריתמים שפתורים בעיית מציאת מסלול קצר ביותר שמניחים שכל הקשתות אי-שליליות (כמו דייקסטרה).
לעומת זאת , יש אלגוריתמים שמאפשרים קשתות שליליים בגרף הקלט , ומחזיר תשובה נכונה כל עוד אין מעגלים שליליים נגישים מקודקוד המקור , אחרת , אם יש מעגל שלילי, האלגוריתם מזהה אותו ומדווח על קיומו .

## מעגלים 
נשאלת השאלה האם מסלול קצר ביותר יכול להכיל מעגל? 
ראשית נראה שמעגל שלילי לא ייתכן במסלול קצר ביותר, נסתכל על הגרף הבא 

![Pasted image 20230114012900.png|300](/img/user/Assets/Pasted%20image%2020230114012900.png)

יש רק מסלול אחד מ $s$ ל $a$ ולכן $\delta(s,a)=3$ ובאופן דומה $\delta(s,b)= -1$ .
נשים לב אבל, שיש אינסוף מסלולים מ $s\to c$ בגלל המעגל , למשל $(s,c) ,(s,c,d,c), (s,c,d,c,d,c)$ וכן הלאה. נשים לב שהמעגל הוא חיובי ולכן המסלול הקצר ביותר לc הוא הקשת הישירה $s\to c$ .

באופן דומה יש אינסוף מסלולים  $s\to e$  אבל במקרה הזה המעגל שלילי ולכן כל סיבוב על המעגל נוכל להקטין את המשקל ולהגיע ל $-\infty$ כלומר , $\delta(s,e) = -\infty$ ובאופן דומה כל קודקוד שניתן להגיע אליו דרך המעגל השלילי ייפגע באופן דומה.

אם כן , כפי שראינו בדוגמה __לא ייתכן שיכיל מעגל שלילי__.

__טענה:__ מסלול קצר ביותר לא יכול להכיל מעגל חיובי.
_הוכחה:_ יהי $P=(v_{0}\dots v_{k})$ מסלול ויהי $C=(v_{i}\dots v_{j})$ מעגל חיובי המוכל ב $P$ , בגלל שהמעגל חיובי, אנחנו יכולים להבטיח שסיבוב על המעגל מעלה את המשקל אם לא היינו משתמשים בו , נשים לב שגם חייב להיות אינדקס $t$ שעבורו $v_{t}$ יהווה את הקודקוד שהקודקוד ה $v_{t+1}$ יוצא מהמעגל או ש $t=k$ כלומר $v_{k}$ הוא בקודקוד היעד שלנו במסלול הקצר ביותר. בשני המקרים נוכל פשוט להגדיר מסלול $P^{\prime}$ על ידי 

$$P^{\prime}= (v_{0}\dots v_{i}\dots v_{t}\dots v_{k})$$

כלומר המסלול המתקבל ממחיקת המעגל והוספת תת המסלול במעגל הזה עד לקודקוד שממנו ממשיכים הלאה לאורך המסלול ב P . נשים לב שמתקיים 
$$w(v_{i}\to v_{t}) < w(C)$$שכן המעגל הוא במשקל חיובי והורדנו את המעגל הזה, בלי הגבלת הכלליות נניח שיש רק את $C$ כמעגל בגרף אחרת פשוט נבצע את אותו תהליך על כל המעגלים ואכן, מצאנו מסלול $P^{\prime}$ ללא מעגלים שיקיים 

$$w(P^{\prime})=w(P) - w(C)   + w(v_{i}\to v_{t}) < w(P)$$

==נשים לב למקרה שבו המעגלים הם ניטרליים כלומר המשקל שלהם הוא 0== - התהליך הנ״ל לא ישתנה שכן עדיין נוכל לקבל מסלול בלי מעגלים קצר ביותר שמשקלו שקול למשקל הגרף עם המעגל הניטרלי.

__מסקנה: מסלול קצר ביותר בגרף $G=(V,E)$ יכיל לכל היותר $|V|$ קודקודים שונים ולכל היותר $|V|-1$ קשתות.__

## ייצוג המסלולים הקצרים ביותר 
לא נרצה רק לחשב משקל המסלול המינימלי , אלא גם לדעת מה הוא .
באופן דומה ל[[Computer Science/Algorithms/BFS\|BFS]] נשמור לכל קודקוד ערך $\pi$ שמייצג את הקודקוד הקודם במסלול, כך נוכל לשחזר את המסלול הקצר ביותר מ $s$ ל $v$ על ידי השיטה הריקורסיבית הבאה

``` psuedo
print_path(G,s,v) 
	if v==s
		print s
	else if v.𝜋 == Nil 
		no path from s to v exists
	else print_path(G,s,v.𝜋)
		print v
```

נרצה אם כן להשתמש ביכולת הזאת להשיג את המסלול בשביל להשיג את הגרף $G_{\pi}=(V_{\pi},E_{\pi})$ שנקרא גם predecessor subgraph שמקיים

$$\displaylines{
V_{\pi}= \{v\in V \ \ | \ \ v.\pi \neq Null\}\cup \{s\} \\
E.\pi = \{(v.\pi , v)\in E \ \ | \ \  v\in V_{\pi}- \{s\} \}
}$$

נרצה להראות עבור אלגוריתם למציאת sssp שערכי ה$\pi$ המתקבלים אחרי ריצתו יקיימו ש $G_{\pi}$ הוא עץ מסלולים קצרים ביותר , כלומר שתת הגרף הוא עץ ששורשו $s$ ומכיל את המסלולים הקצרים ביותר מ$s$ לכל $v\in V$ שנגיש מ $s$ .

_באופן פורמלי ננסח זאת כך:_ יהי $G=(V,E)$ גרף מכוון ממושקל עם פונקציית משקל $w$ . נניח ש $G$ לא מכיל מעגלים שליליים נגישים מקודקוד המקור $s$ .
עץ המסלולים הקצרים ביותר ששורשו $s$ יהיה תת גרף מכוון $G^{\prime} =(V^{\prime},E^{\prime})$ המקיימים
א) $V^{\prime}$ הוא קבוצת הקודקודים הנגישים מ $s$
ב) $G^{\prime}$ מהווה עץ ששורשו $s$ 
ג) לכל $v\in V$ המסלול הפשוט מ $s$ ל $v$ ב $G^{\prime}$ הוא המסלול הקצר ביותר ב $G$ .

==נשים לב שהמסלולים לא בהכרח ייחודיים, וכך גם עץ המסלולים הקצרים ביותר אינו ייחודי למשל:==
![Pasted image 20230114140515.png|400](/img/user/Assets/Pasted%20image%2020230114140515.png)

## Relaxation
האלגוריתם שנראה בהמשך, משתמש בטכניקה שנקראת ״הקלה״ או [Relaxation](https://en.wikipedia.org/wiki/Relaxation_(approximation)) בלעז.
לכל קודקוד $v\in V$ שומרים ערך $d$ שמהווה חסם עליון על משקל המסלול המינימלי מ $s$ קודקוד המקור אליו. אם כן נגדיר ראשית פונקצייה בסיסית שכל תפקידה הוא לאתחל את החסם העליון הזה

``` psuedo
initialize_single_source(G,s):
	for each vertex v in G.V
		v.d = INFINITY
		v.𝜋 = NIL
	s.d = 0
```

כעת תהליך ההקלה של קשת $(u,v)$ כלשהי היא בדיקת אם אנחנו יכולים לשפר את המסלול הקצר ביותר ל$v$ שמצאנו עד כה ע״י הקשת $(u,v)$. במידה וכן נעדכן את $v.d, v.\pi$ בהתאמה.
אם כן נשים לב שהתהליך של ההקלה יכול רק להקטין את הערך $d$ ולשנות את הקודקוד מקום של $v$ במסלול הקצר ביותר (כולם nil בהתחלה כי אנחנו לא מכירים את המסלולים בתחילת האלגוריתם).

``` psuedo
relax(u,v,w)
	if v.d > u.d + w(u,v)
		v.d = u.d + w(u,v)
		v.𝜋 = u
```

![Pasted image 20230115123717.png|300](/img/user/Assets/Pasted%20image%2020230115123717.png)
האלגוריתם הגנרי שנראה בהמשך ישתמש בשתי הפונקציות שבנינו , בראשונה בתחילת האלגוריתם ובהקלה הוא ישתמש שוב ושוב עד לסיום התהליך כלומר עד שלא ניתן להקטין יותר את d. האלגוריתמים נבדלים זה מזה בכמות הפעמים שקוראים לפונקציית הההקלה עבור כל קשת וכמובן הסדר שבו מבצעים את ההקלות.

### תכונות של relaxation

א) __אי שיוויון המשולש:__

$$\forall_{(u,v)\in E}: \delta(s,v)\leq \delta(s,u)+ w(u,v)$$

ב) __תכונת החסם העליון:__ 
תמיד מתקיים

$$\forall_{v\in V}: v.d\geq \delta(s,v)$$

וכאשר $v.d$ מגיע ל $\delta(s,v)$ הוא לא ישתנה (יקטן יותר).

_הוכחה:_ באינדוקצייה על מספר פעולות ההקלה .

בסיס- לאחר האתחול של מקור יחיד עבור $s$ כלומר ביצענו 0 הקלות עד כה מתקיים שכל הקודקודים שאינם $s$ הערך של d יהיה $\infty$ ושל $s$ יהיה $0$ מה שמקדיים את הדרוש.

צעד- נניח שלפני ביצוע `relax((u,v),w)` מתקיים שלכל הקודקודים ב $V$ $d.v\geq(s,v)$ מהנחת האינדוקצייה.
פעולת ההקלה יכולה לשנות רק את הערך של $v.d$ ולכן כל האחרים לא ישתנו. כמו כן,  פונקציית ההקלה תקיים רק 

$$v.d= u.d+ w(u,v)\geq \delta(s,u)+ w(u,v)\geq \delta(s,v)$$


ג) __חוסר המסלול__:
אם אין מסלול מ $s$ ל $v$ אז יתקיים 

$$v.d = \delta(s,v)=\infty$$

הוכחה- אם אין מסלול מ $s$ לקודקוד $v$ כלשהו אז $\delta(s,v)=\infty$ כבר בשלב האתחול יתקיים 
$v.d=\delta(s,v)=\infty$ ולפי הטענה הקודמה ברגע שהגענו לשיוויון עם משקל המסלול הקצר ביותר זה לא אמור לקטון יותר __כדרוש__.

ד) __התכנסות:__
אם המסלול $s\to u,v$ הוא המסלול הקצר ביותר ב $G$ עבור $v\in V$ וגם $u.d=\delta(s,u)$ בזמן מסויים עקב פעולת relax על $(u,v)$ , אזי $v.d = \delta(s,v)$ בכל זמן אחרי כן.

ההוכחה נובעת מתכונת החסם העליון תמיד יקיים $v.d\geq \delta(s,v)$ אם לפני ההקלה יש שיוויון אז סיימנו.
אחרת, לפני ההקלה יתקיים 

$$v.d>\delta(s,v)= \delta(s,u)+w(u,v)= u.d+w(u,v)$$

ולאחר ההקלה יתקיים 

$$v.d= u.d+w(u,v)$$

והשיוויון מתקיים כדרוש.

ה)  __path relaxation:__
אם $P=(v_{0}\dots v_{k})$ המסלול הקצר ביותר מ $s=v_{0}$ ל $v_{k}$ ועושים הקלה לקשתות $P$ לפי הסדר במסלול אזי 

$$v_{k}.d =\delta(s,v_{k})$$

בלי קשר האם עשינו פעולות הקלה נוספות על קשתות שלא במסלול.

גם את הטענה הזאת אפשר להוכיח באינדוקצייה על מספר ההקלות בתת המסלול.
_בסיס-_ עבור $i=0$ לפני ההקלה הראשונה מתקיים 

$$s.d=\delta(s,s)=0$$

ומטענת החסם העליון זה לא ישתנה לאורך כל התוכנית.
_צעד-_ נניח שלפני ההקלה  $relax(v_{i-1},v_{i},w)$ לכל $j<i$ מקיים 

$$v_{j}.d=\delta(s,v_{j})$$

בגלל שזה נכון מהנחת האינדוקצייה בפרט עבור $v_{i-1}$  מתכונת ההתכנסות למעלה יתקיים שלאחר ההקלה

$$v_{i}.d=\delta(s,v_{i})$$

ו) __predecessor subgraph__
כאשר $v.d=\delta(s,v)$ לכל קודקוד בגרף אז $G_{\pi}$ הוא עץ המסלולים הקצרים ביותר ששורשו $s$ .

==נשים לב שהמשפטים הללו נכונים רק בהנחה שהאלגוריתם מתחיל בפונקציית האתחול ושינוי של הערכים לאחר מכן נעשה רק בפונקציית ההקלה==

## Bellman-Ford Algorithm
האלגוריתם פותר את בעיית SSSP במקרה הכללי, שבו ייתכנו קשתות שליליות.
_קלט:_ גרף מכוון $G=(V,E)$ עם פונקציית משקל $w$ וקודקוד מקור $s\in V$ 
_פלט:_ True אם אין מעגלים שליליים, ועבור כל קודקוד $v\in V$ ו False אם יש מעגלים שליליים

``` psuedo
bellman_ford(G,w,s)
	initialize_single_source(G,s)
	for i=1 to |V|-1:
		for each (u,v) in E
			relax(u,v,w)
			
	\\check for negative cycles
	for each edge (u,v) in E
		if v.d > u.d + w(u,v)
			return FALSE
	return TRUE
```
הרעיון בשלב הבדיקה אם נשים לב לתנאי, זה בעצם ביצוע הקלה נוספת, מתכונה (ה) שציינו למעלה לאחר ההקלה הנוספת הערך של קודקוד כלשהו לא אמור להשתנות בהינתן שכבר יש לנו מסלול קצר ביותר, אם זה אכן השתנה, אז בוודאות יש מעגל שלילי שבהקלה נוספת שוב יקטין את הערך עד ל $-\infty$ .

![Pasted image 20230115013416.png|400](/img/user/Assets/Pasted%20image%2020230115013416.png)

### זמן הריצה 
האתחול בשורה 1 עולה $O(V)$. כל אחת מ $|V|-1$ האיטרציות עולה $\Theta(E)$ והלולאה הראשונה עולה $O(VE)$ ולולאת הבדיקה עבור מעגלים שליליים היא $O(E)$ 
סה״כ  $O(VE)$

### הוכחת נכונות
בשביל להתחיל להוכיח נכונות של האלגוריתם נוכיח את הלמה הבאה
יהי $G=(V,E)$ גרף מכוון, ממושקל עם $w: E\to\mathbb{R}$ וקודקוד מקור $s\in V$ .
אחרי $|V|-1$ איטרציות של לולאת for הראשונה יתקיים $v.d= \delta(s,v)$ .

נחלק למקרים: 
אם הקודקוד לא נגיש אז אנחנו יודעים שאין מסלול קצר ביותר ולכן $v.d = \delta(s,v)=\infty$ .
אם הקודקוד נגיש מ $s$ ניקח את המסלול הקצר ביותר $s\to v$ ונסמנו

$$P=(s,v_{1},v_{2},\dots, v)$$

כפי שכבר טענו במסלול קצר ביותר אין מעגלים כלומר הוא מסלול פשוט ולכן אורכו $|V|-1$ לכל היותר כלומר 

$$|P|= k\leq |V|-1$$

אם כן, יתקיים שלכל $1\leq i\leq k$ באיטרציה ה$i$ של הלולאה החיצונית נבצע הקלה בפרט על הקשתות $(v_{i-1}, v_{i})$ . מטענה שתת מסלול של מסלול קצר ביותר הוא גם כן הקצר ביותר נקבל שתמיד ההקלה על הקשת הנ״ל תיצלח כלומר התנאי יתקיים ונשנה את הערך $v_{i}.d$. 
סה״כ על פי למת ה path relaxation מלמעלה נקבל

$$v.d = v_{k}.d = \delta(s,v_{k})= \delta(s,v)$$

==המסקנה המתבקשת היא שיש מסלול קצר ל v מ s ביותר לאחר האלגוריתם אם ורק אם הערך v.d קטן מ אינסוף==

__כעת נוכל להשתמש בלמה הנ״ל והתכונות האחרות להוכחת הנכונות__ 
נניח ש $G$ לא מכיל מעגלים שליליים נגישים מ $s$ , בסוף הריצה, מתקיים 
$$v.d = \delta(s,v)$$
ועל פי טענת predecessor subgraph נובע ש $G_{\pi}$ הוא עץ המסלולים הקצרים ביותר.
כל שנשאר לעשות הוא להראות שהאלגוריתם מחזיר true
לכל $(u,v)$ מתקיים בסיום הריצה
$$v.d = \delta(s,v)\leq \delta(s,u)+w(u,v)= u.d+w(u,v)$$
לכן אם אין מעגלים שליליים מאי שיוויון המשולש למעלה לעולם לא נכנס לשורה שמחזירה false, כלומר נחזיר true.

כעת אם $G$ מכיל מעגלים שליליים נסמן את אחד מהם $C=(v_{0}\dots v_{k})$ כמובן ש $v_{0}=v_{k}$ ואז

$$w(C)=\sum\limits_{i=1}^{k}w(v_{i-1},v_{i})<0$$

כעת נניח בשלילה שהאלגוריתם מחזיר true אם כן:

$$\forall_{1\leq i\leq k}: v_{i}.d\leq v_{i-1}.d +w(v_{i-1},v_{i})$$

כיוון שהתנאי כדי להחזיר false לא מתקיים לאף קשת ופרט לכל קשתות המסלול C . 

$$\sum\limits_{i=1}^{k}v_{i}.d\leq \sum\limits_{i=1}^{k}(v_{i-1}.d+w(v_{i-1},v_{i}))=\sum\limits_{i=1}^{k}v_{i-1}.d+\sum\limits_{i=1}^{k}w(v_{i-1},v_{i})$$

כיוון ש $v_{0}=v_{k}$ 

$$\sum\limits_{i=1}^{k}v_{i}.d= v_{0}.d+\sum\limits_{i=1}^{k-1}v_{i}.d= \sum\limits_{i=1}^{k}v_{i-1}.d$$

כלומר קיבלנו 

$$\sum\limits_{i=1}^{k}v_{i}.d= \sum\limits_{i=1}^{k}v_{i-1}.d\leq \sum\limits_{i=1}^{k}v_{i-1}.d+\sum\limits_{i=1}^{k}w(v_{i-1},v_{i})  $$

וזה ייתכן רק אם $\sum\limits_{i=1}^{k}w(v_{i-1},v_{i}) =0$ בסתירה לכך שהוא מעגל שלילי.
## SSSP in DAG
עבור גרף מכוון חסר מעגלים (DAG) אלגוריתם Bellman-Ford פותר את בעיית SSSP עבורו בזמן $O(VE)$. נרצה לשפר את זמן הריצה ולנצל את הנתון החדש על הגרף שהוא DAG.

נתאר אלגוריתם שממיין את הקודקודים על ידי [[Computer Science/Algorithms/DFS#מיון טופולוגי\|מיון טופולוגי]] ואז מאתחלים מהקודקוד הראשון בסידור הטופולוגי, עבור כל קשת יוצרת נבצע הקלה. נזכר שאם קיין מסלול מ $u$ ל $v$ בגרף אז $u$ מופיע לפניו בסדר הטופולוגי.

``` psuedo
DAG_SSSP(G,w,s)
	topologically_sort(G)
	initialize_single_source(G,s)
	for i=1 to |V| 
		for each u in adj(v_i)
			relax(u,v_i,w)
```

הרעיון כאן הוא שאנחנו יודעים בידיוק מיהן הקשתות שיושפעו מההקלה ולכן לא צריך לבצע את ההקלטה על כל הקשתות אלא רק על השכנים במצב של מיון טופולוגי, זה מייעל מאוד את האלגוריתם כפי שנרצה בניתוח זמן הריצה.

![Pasted image 20230115123918.png](/img/user/Assets/Pasted%20image%2020230115123918.png)

### זמן ריצה 
מיון טופולוגי לוקח $O(V+E)$ , האתחול הוא $O(V)$ והלולאה כעת למרות שזאת לולאה כפולה , רצה רק על השכנים של אות והקודקוד שאנחנו עליו באיטרצייה ה$i$ ולכן סך הכל תרוץ על כל הקשתות בלי כפילויות כלומר  $O(|E|)$ עבור __כל הקודקודים__ . סך הכל 
$$O(|V|+|E|)$$
### הוכחת נכונות
יהי $G=(V,E)$ גרף מכוון חסר מעגלים ממושקל עם $w$. בסיום הרצת האלגוריתם הנ״ל על $G$ עם קודקוד מקור $s\in V$ יתקיים

$$\forall_{v\in V}: v.d = \delta(s,v)$$

וגם $G_{\pi}$ הוא עץ המסלולים קצרים ביותר.

נראה קודם ש  $\forall_{v\in V}: v.d = \delta(s,v)$  . נחלק למקרים 
אם $v$ לא נגיש מקודקוד המקור, ע״פ למת אין מסלול מתקיים $v.d=\delta(s,v)=\infty$ .
אם $v$ נגיש מ $s$  , אז קיים מסלול קצר ביותר מ $s$ ל $v$ נסמנו $P$ ונניח $|P|=k$ . כיוון שעוברים על הקשתות בסדר הטופולוגי אנחנו נבצע הקלות __לפי סדר המסלול__ ועל פי הלמות של ההקלה יתקיים

$$\forall_{0\leq i\leq k}: v_{i}.d= \delta(s,v_{i})$$

בסוף לפי למת ה Predecessor subgraph מתקיים כתוצאה מהנ״ל שאכן קיבלנו עץ מסלולים קצר ביותר עבור  $G_{\pi}$  .

## Dijkstra’s Algorithm
בהנחה ופונקציית המשקל היא אי שלילית כלומר $w: E\to\mathbb{R}^{+}$ אז נוכל להשתמש באלגוריתם דייקסטרה שהוא יעיל יותר כדי לפתור את בעיית SSSP על גרף מכוון ממושקל $G=(V,E)$.

רעיון האלגוריתם הוא להחזיק קבוצה $S$ של קודקודים שהמשקל הסופי למסלול המינימלי מ $s$ __כבר נקבע__ האלגוריתם שוב ושוב ייקח $u\in V-S$ עם הערך $d$ המינימלי , מכניס אותו ל $S$ ומבצע הקלה לכל הקשתות שיוצאות מ $u$ . כדי לשלוף את המינימום בצורה יעילה ננהל תור קדימויות ביחס לערכי $d$ .

``` psuedo
dijkstra(G,w,s)
	initialize_single_source(G,s)
	S = {}
	Q.init(G.V, using: V.d)
	while Q.is_not_empty:
		u = Q.extract_min()
		S = S.union(u)
		for each v in adj(u):
			relax(u,v,w)
```

קצת לא אינטואיטיבי לשים לב לזה אבל בשלב הראשון הקודקוד שיצא מהמינימום יהיה $s$ כיוון שבאתחול הוא היחיד שמקבל $d=0$ .

__נשים לב שיש כאן אלמנט של בחירה חמדנית, שהיא לקחת כל פעם את הקודקוד עם ערך d מינימלי, כלומר זה [[Computer Science/Algorithms/greedy algorithms\|אלגוריתם חמדן]]__

### זמן ריצה
עבור $Q$ מבצעים שלוש פעולות: init, extract, decrease_key כאשר השלישי זאת בעצם פעולת ההקלה שעושים. האלגוריתם קורא לextract בידיוק פעם אחת לכל קודקוד.
כיוון שכל קודקוד נכנס פעם אחת ל $S$ כל קשת ברשימת השכנויות $adj[u]$ נבדקת פעם אחת בלולאה במהלך הריצה, אז הלולאה תרוץ $|E|$ בסך הכל ותפעיל את ההקלה.
אם כן, זמן הריצה תלוי במימוש $Q$

א) _מערך:_  $O(|V|^{2})$
ב) _ערימה בינארית:_ $O(V\log V + E\log V)$ 
ג) ערימת פיבונאצי : $O(E+V\log V)$ 

### נכונות 
כפי שאמרנו האלגוריתם הוא חמדני וניתן להוכיח אותו עם למות הבחירה, עם זאת, ארצה להראות שברגע ש $u$ מצטרף ל $S$ : $d[u]= \delta(s,u)$ בגלל שכל הקודקודים נכנסים ל $S$ בסוף האלגוריתם זה יוכיח את נכונותו. 
נב״ש כי זה לא מתקיים ונסמן ב $u$ את הקודקוד הראשון שלא מקיים זאת.
ניעזר בתכונה [invariant](https://en.wikipedia.org/wiki/Invariant_(mathematics)#Invariants_in_computer_science) של האלגוריתם , אינוריאנטה זאת תכונה שלא משתנה לאורך כל ריצת האלגוריתם, במקרה הזה  התכונה הזאת תהיה שכל פעם שמתחילים לולאת while באלגוריתם יתקיים עבור כל $v\in S$ ש $d[v]=\delta(s,v)$ . נרצה להראות שהדרך היחידה ש$u$ הנ״ל יצטרף ל $S$ זה אם הוא יקיים את התכונה הזאת בסתירה להנחה שלנו. נעשה זאת באינדוקצייה __על מספר הפעמים שמגיעים לwhile באלגוריתם__ 

_בסיס:_ 
בפעם הראשונה שמגיעים ללולאת while מתקיים $S=\emptyset$ ולכן התנאי מתקיים באופן ריק.

_צעד:_
ניקח את קודקוד $u$ הנ״ל שהצטרף ל $S$ אך לא מקיים את האינווריאנטה. כמו כן , עדיף לנו, לשם הנוחות להניח בלי הגבלת הכלליות שזאת הפעם הראשונה שהתנאי לא מתקיים. אנחנו יודעים ש $u\neq s$ כי עוד באתחול אנחנו נותנים ל $s$ את המרחק המינימלי המתאים לו, $0$ וזה לא משתנה לאורך כל האלגוריתם. כמו כן העובדה שאנחנו אומרים ש $u.d\neq \delta(s,u)$ מעידה על כך שקיים מסלול בין $s$ ל $u$ אחרת באתחול היינו מקבלים $u.d =\delta(s,u)=\infty$ ומהתכונות שהראנו על ההקלות זה לא היה משתנה גם כן.
ניקח את המסלול הקצר ביותר בינהם ונסמנו $P^{\prime}$ . נסמן ב $y$ את הקודקוד הראשון במסלול זה שלא שייך ל$S$ ונסמן ב$x$ את הקודקוד שקדם ל$y$ במסלול. 
כאן נכנסת לתמונה העובדה שבחרנו ב$u$ להיות הקודקוד הראשון ששובר את הוריאנטה. אנחנו יודעים $x.d = \delta(s,x)$ כי הוא כבר בתוך $S$ .
לפני ש $x$ נכנס ל $S$ בוצעה הפונקצייה 

$$relax(x,y,w)$$

ומתכונת התכנסות המסלול אנחנו יודעים שלאחר פעולת ההקלה יתקיים גם 

$$y.d=\delta(s,y)$$

כעת, ניקח את הרישא של $P^{\prime}$ שמסתיימת ב $y$ נסמנה $P$ . אנחנו יודעים מהעובדה שתת מסלול של מסלול קצר ביותר , הוא גם כן קצר ביותר ש:

$$\delta(s,y) = w(P)\leq w(P^{\prime})= \delta(s,u)$$

וגם בחרנו את $u$ לפני $d$ ולכן

$$u.d\leq y.d$$

וסך הכל נקבל

$$y.d=\delta(s,y)\leq \delta(s,u)\leq d.u\leq d.y$$

המצב היחיד שזה יתקיים היא אם $u.d=\delta(s,u)=\delta(s,y)$ בסתירה!
