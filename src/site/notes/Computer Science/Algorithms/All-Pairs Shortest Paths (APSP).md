---
{"dateCreated":"2023-02-04 15:01","tags":["algorithms","computer_science"],"pageDirection":"rtl","dg-publish":true,"permalink":"/computer-science/algorithms/all-pairs-shortest-paths-apsp/","dgPassFrontmatter":true}
---


# APSP

אלגוריתם APSP הוא אלגוריתם למציאת מסלולים קצרים ביותר בין כל הזוגות בגרף . סוג של הכללה לבעית ה[[Computer Science/Algorithms/Single-Source Shortest Paths (SSSP)\|SSSP]].

__קלט:__ גרף $G=(V,E)$ ופונקציית משקל $w:E\to\mathbb{R}$ . לשם הנוחות בהגדרות נסמן את קבוצות הקודקודים על ידי 

$$V=\{1,2,3\dots ,n\}$$

פשוט $V_{i}=i$ ...

__פלט:__ נרצה להחזיר שתי מטריצות - 

א) מטריצת מרחקים $D^{|V|\times|V|}$ כך ש $d_{i,j}=\delta(i,j)$ 
ב) מטריצת קודמים  $\Pi^{|V|\times|V|}$  כך ש $\forall_{i=j\text{ or }\delta_{i,j}=\infty}:\pi_{i,j}=NULL$ ואחרת $\pi_{i,j}$ זה הקודקוד הקודם ל $j$ במסלול הקצר ביותר בין $j$ ל $i$.

>[!info] קו מנחה:
>נשים לב שגם בבעיית המסלולים הקצרים ביותר מקודקוד יחיד התמקדנו בגרפים ללא מעגלים שליליים וכך זה ימשיך פה, נוכל להוסיף לאלגוריתם ״שכבת הגנה״ שבודקת התכנות של מעגלים שליליים בגרף ואם אכן יש כאלו נחזיר שגיאה כלשהי ולא פלט רצוי.

## פתרון נאיבי באמצעות sssp 
ניתן להריץ את האלגוריתם של [[Computer Science/Algorithms/Single-Source Shortest Paths (SSSP)#Bellman-Ford Algorithm\|בלמן-פורד]] מכל קודקוד כלומר בזמן 
$$O(|V|\cdot\text{ bellman ford runtime})=O(|V|^{2}\cdot|E|)$$
_אם אנחנו יודעים שהקשתות אי שליליות_ - נוכל להריץ מכל קודקוד את האלגוריתם של דייקסטרה ולקבל זמן ריצה עדיף . לפי מבנה הנתונים שהשתמשנו בו כדי להחזיק את קשת החתך המינימלי כאיבר מינימום.

א) _מערך:_  $O(|V|^{3})$
ב) _ערימה בינארית:_ $O(V^{2}\log V + VE\log V)$ 
ג) ערימת פיבונאצי : $O(VE+V^{2}\log V)$ 

> [!info] חסם על מספר הקשתות
> לעתים נרצה לבטא את זמן הריצה ככזה שתלוי רק במשתנה אחד למשל מספר הקודקודים, נזכר שישנו חסם על מספר הקשתות בגרף.
> א) אם הגרף דליל sparse- $|E|=\Theta(|V|)$ 
> ב) אם הגרף צפוף dense- $|E|=\Theta(|V|^{2})$
> 
> ![Screenshot 2023-02-04 at 15.31.30.png](/img/user/Assets/Screenshot%202023-02-04%20at%2015.31.30.png)

>[!info] הבחנה
>אם הגרף לא ממושקל נוכל להריץ [[Computer Science/Algorithms/BFS\|BFS]] על כל קודקודי הגרף ולפתור את בעיית APSP בזמן ש ל$O(V^{2}+VE)$ 

## אלגוריתם פלויד וורשאל
אלגוריתם המשתמש ב[[Computer Science/Algorithms/Dynamic Programming\|תכנות דינמי]] . נרצה להגדיר נוסחת נסיגה לכל מסלול שכל פעם מתבססת על קלט ״קטן״ יותר של קודקודים בהם ניתן להעזר.

__קודקודי ביניים:__ יהי $p=(v_{1},v_{2}\dots v_{k})$ מסלול.  נגדיר את $\{v_{2},\dots,v_{k-1}\}$ כקודקודי הביניים של $p$ .

נרצה להבין איך מגדירים את נוסחת הנסיגה של האלגוריתם הריקורסיבי ובעצם מהו הפתרון האופטימלי...
לשם הנוחות נניח שקודקודי הגרף ממוספרים (כפי שהגדרנו בהתחלה) ונתבונן בקבוצת הקודקודים 

$$\{1,2,\dots,k\}$$

כעת נתבונן __בכל__ המסלולים משתי קודקודים $i,j$ כך שכל קודקודי הביניים שלהם הם מהקבוצה $\{1,\dots,k\}$ 
יהי $p$ המסלול הקצר ביותר _(שאנחנו יודעים שבגלל שאין מעגלים שליליים הוא מסלול פשוט)_ מבין כל המסלולים הנ״ל שלקחנו, $p$ הוא מהצורה:

![Pasted image 20230204163707.png|300](/img/user/Assets/Pasted%20image%2020230204163707.png)

נשים לב לשתי מקרים חשובים שיכולים להיות עבור $k$ :
- __אם $k$ אינו קודקוד ביניים של $p$__ אזי כל קודקודי הביניים הם מהקבוצה $\{1,2,\dots,k-1\}$ ולכן, המסלול הקצר ביותר מ $i$ ל $j$ עם קבוצת קודקודי הביניים $\{  1,2\dots,k-1\}$ הוא אותו המסלול $p$ (באופן ריקורסיבי נוכל למצוא את אורכו).

- __אם $k$ קודקוד ביניים של $p$__ אזי נוכל לחלק את $p$ להיות מהצורה

$$p= i\overset{p_{1}}{\rightsquigarrow} k\overset{p_{2}}{\rightsquigarrow} j$$

![Pasted image 20230204164708.png|350](/img/user/Assets/Pasted%20image%2020230204164708.png)

כעת יש לנו 2 מסלולים $p_{1,2}$ ש $k$ אינו קודקוד ביניים בהם אבל הם מכילים רק קודקודים מהקבוצה $\{1,2,\dots,k-1\}$ שהיא גם כן קבוצת קודקודי ביניים. אנחנו יודעים שתת מסלול של מסלול קצר ביותר הוא גם כן מסלול קצר ביותר כלומר $p_{1}$ הוא המסלול הקצר ביותר מ $i$ ל $k$ שמשתמש בקבוצת קודקודי הביניים הנ״ל. ובאופן דומה $p_{2}$ הוא המסלול הקצר ביותר מ $k$ ל $j$ שמשתמש בקבוצת קודקודי הביניים הנ״ל. 

### פתרון ריקורסיבי 
נגדיר $d_{i,j}^{(k)}$ להיות אורך המסלול הקצר ביותר מ $i$ ל $j$ מבין המסלולים שקבוצת קודקודי הביניים שלהם היא $\{1,2,\dots k\}$ . __נבחין:__ ש $d_{i,j}^{(k)}$ הוא איבר מהמטריצה $D^{(k)}$ שהיא חלק מקבוצת המטריצות 

$$\{D^{(i)} \ \ | \ \ i\in [0,n]\}$$

כל המטריצות הן מטריצות מרחקים עבור המסלול עם קבוצת קודקודי הביניים $\{1\dots i\}$ . כאשר $i=0$ המשמעות היא שאין קודקודי ביניים כלומר היא תכיל את המרחקים בין כל הקודקודים $i,j$ שיש בינהן קשת ישירה או שאין ואז הערך הוא $\infty$ .

__איך מאכלסים את $d_{i,j}^{k}$ ?__ 
נוכל לאכלס כל איבר במטריצות הנ״ל באופן הבא, בזכות למת תת המסלול...

$$d_{i,j}^{(k)}=\begin{cases}
 w_{i,j}& k=0\\ \min(d_{i,j}^{(k-1)}, d_{ik}^{(k-1)}+d_{kj}^{(k-1)})  &k\geq 1\\
\end{cases} $$

כלומר המינימום בין המקרה ש $k$ הוא קודקוד ביניים למקרה שהוא לא קודקוד ביניים, מהנ״ל אחד מהם חייב להתקיים. כך ממשיכים עד שנוכל לקחת באופן ישיר את משקל הקשת.

>[!info] הבחנה:
>כיוון שלכל מסלול קודקודי הביניים הם מהקבוצה $V=\{1,\dots, n\}$ המטריצה $D^{(n)}$  תכיל את הפתרון הסופי: 
>
>$$d_{ij}^{(n)}=\delta(i,j)$$

### תכנות דינמי
הקלט יהיה מטריצת שכנויות $W$ שתקיים 

$$D^{(0)}= W$$

כלומר במקום לשים ערך בינארי במטריצה , היכן שיש קשת נשים את משקלה. החישוב אם כן, יהיה מההתחלה לסוף, על כל $D^{(i)}$ נחשב את $D^{(i+1)}$  עד שנגיע ל $D^{(n)}$ .

``` psuedo
floyd_warshall(W):
	n = W.rows
	D[0] = W
	for k = 1 to n 
		D[K] = new matrix[n,n]
		for i = 1 to n
			for j = 1 to n
				d_ij[k] = min(d_ij[k-1], d_ik[k-1]+ d_kj[j-1])
```

זמן ריצה $O(|V|^{3})$ .

__שחזור פתרון:__
ניתן לחשב את מטריצת הקודמים $\Pi$ תוך כדי חישוב האלגוריתם של $D^{(k)}$ . 
יתקיים 

$$\Pi = \Pi ^{(n)}$$
ונחשב את מטריצת הקודמים בסדר עולה 

$$\Pi^{(0)}\dots \Pi^{(n)}$$
נגדיר $\pi_{ij}^{(k)}$ להיות האבא של קודקוד $j$ במסלול קצר מ $i$ כאשר כל קודקודי הביניים הם מהקבוצה $[k]$ .
מקרה הבסיס הוא $k=0$ כלומר אין קודקודי ביניים ויתקיים

$$\pi_{ij}^{(0)}=\begin{cases}
\text{null} & i=j\text{ or } w_{ij}=\infty  \\
i & i\neq j \text{ and } w_{ij}< \infty
\end{cases}$$
בכל מצב אחר :

$$\forall_{1\leq k\leq n}:\pi_{ij}^{(k)}=\begin{cases}
\pi_{ij}^{(k-1)} & d_{ij}^{(k-1)}\leq d_{ik}^{(k-1)}+d_{kj}^{(k-1)} \\
\pi_{kj}^{(k-1)} & else
\end{cases}$$
נשלב את הפונקצייה בפתרון הנ״ל וזה לא יפריע לזמן הריצה.

![Screenshot 2023-02-04 at 18.22.38.png|400](/img/user/Assets/Screenshot%202023-02-04%20at%2018.22.38.png)

## האלגוריתם של ג'ונסון 
האלגוריתם של גונסון פותר את בעיית $APSP$ בזמן $O(|V|^{2}\log|V|+|V|\cdot |E|)$ שזה נחשב שיפור משמעותי אם הגרף דליל..

__דליל__ - $O(V^{2}\log V)$ 
__צפוף__ - $O(V^{2}\log V + V^{3})= O(V^{3})$

כמו כן, האלגוריתם של ג׳ונסון מזהה מעגלים שליליים בגרף במידה והיה. 
הוא מתבסס על האלגוריתמים של דייקסטרה ובלמן פורד ומשתמש בטכניקה שנקראת [שקלול מחדש](https://arogozhnikov.github.io/hep_ml/reweight.html).

>[!info] הבחנה:
>אם ידוע שכל קשתות הגרף אי שליליות אפשר עדיין להריץ את דייקסטרה על כל קודקודי הגרף ולקבל זמן ריצה שקול.

כלומר מההבחנה הזאת , והעובדה שציינו שאנחנו הולכים להשתמש בטכניקה שנקראת שיקלול מחדש, אפשר כבר להבין את הכיוון שאליו אנחנו הולכים. נרצה לחשב פונקציית משקלים חדשה עבור הקשתות של הגרף ששומרת על היחסים המקוריים __אבל__ תגרום לכל הקשתות להיות אי שליליות כדי שנוכל להריץ דייקסטרה. 

באופן פורמלי נרצה להגדיר $\hat{w}$ שתקיים שתי תנאים:
א) $\hat{w}:E\to\mathbb{R}^{+}$ - כלומר פונקצייה אי שלילית.
ב) $w(u \rightsquigarrow  v)=\delta(u,v)\leftrightarrow \hat{w}(u \rightsquigarrow  v)=\hat{\delta}(u,v)$ כלומר, מסלול קצר ביותר לפי פונקציית המשקל המקורית חייב להיות מסלול קצר ביותר לפי פונקציית המשקל החדשה שנגדיר.

__נבנה כמה לימות עזר שבאמצעותן נוכל להגדיר פונקציית משקלים חדשה__:

==למה 1== שקלול מחדש לא משנה את המסלולים הקצרים:
יהי $G=(V,E)$ גרף מכוון ממושקל עם $w:E\to\mathbb{R}$ ותהי $h:V\to\mathbb{R}$ פונקצייה כלשהי שממפה קודקודים למספרים ממשיים. 
לכל קשת $(u,v)\in E$ נגדיר

$$\hat{w}(u,v)= w(u,v)+h(u)-h(v)$$

יהי $p=(v_{0},v_{1}\dots, v_{k})$ מסלול כלשהו מ $v_{0}$ ל $v_{k}$ אז:

$$w(u \rightsquigarrow  v)=\delta(u,v)\leftrightarrow \hat{w}(u \rightsquigarrow  v)=\hat{\delta}(u,v)$$

בנוסף, $G$ יכיל מעגלים שליליים לפי $w$ אמ״מ $G$ מכיל מעגלים שליליים לפי $\hat{w}$ .

__הוכחה:__

$$\displaylines{
\hat{w}(p) = \sum\limits_{i=1}^{k}\hat{w}(v_{i-1},v_{i})\\
= \sum\limits_{i=1}^{k}w(v_{i-1},v_{i})+h(v_{i-1})-h(v_{i})\\
= \sum\limits_{i=1}^{k} w(v_{i-1},v_{i})+h(v_{0})-h(v_{k})\\
= w(p)+h(v_{0})-h(v_{k})
}$$

הראנו תכונה מעניינת על הפונקצייה החדשה שאנחנו מגדירים כאשר מפעילים אותה על המסלול. בגלל שיש מעין טלסקופיות בטור הנ״ל. לא משנה מיהו המסלול $p$ כל עוד הוא מתחיל בקודקודים $v_{0}$ ונגמר ב $v_{k}$ תמיד נסכום ונחסיר את הפעלת $h$ על הקודקודים האלו. בפרט זה נכון על המסלול הקצר ביותר מ $v_{0}$ ל $v_{k}$ וכיוון שאלו לא תלויים במסלול אזי יתקיים שאם $p$ הוא המסלול הקל ביותר בין $v_{0}$ ל $v_{k}$ לפי $w$ אז ברור שגם $p$ יהיה המסלול הקל ביותר לפי $\hat{w}$ .

נשים לב שאם $c=(v_{0},\dots,v_{k})$ מעגל, אזי :

$$\hat{w}(c)= w(c)+h(v_{0})-h(v_{k})= w(c)+h(v_{0})-h(v_{0})= w(c)$$

כלומר המשקלים של כל המעגלים זהים ובפרט אלו השליליים. 

==למה 2== יצירת משקלים אי שליליים באמצעות שקלול מחדש:
בהינתן גרף מכוון $G=(V,E)$ משוקלל עם $w:E\to\mathbb{R}$ , נבנה גרף חדש $G^{\prime}=(V^{\prime},E^{\prime})$ כאשר 

$$\displaylines{
V^{\prime}= V\cup \{s\} \ \ \ \  \text{ where: } s\notin V \\
E^{\prime}= E\cup\{(s,v)  \ \ | \ \ v\in V\} \\
\forall_{v\in V} : w(s,v)=0
}$$

כעת נניח ש $G,G^{\prime}$ לא מכילים מעגלים שליליים, נגדיר $h(v)=\delta(s,v)$ לכל $v\in V^{\prime}$ . 
_נרצה להוכיח_ : $\hat{w}\geq 0$ כלומר זאת פונקצייה אי שלילית.

__הוכחה:__ 
ראשית נבחין שכיוון שאין קשתות נכנסות ל $s$ , אז הוא לא יופיע במסלולים שלא מתחילים מ $s$ כמו כן מהגדרת הגרף החדש יתקיים ש $G^{\prime}$ מכיל מעגלים שליליים אמ״מ $G$ מכיל מעגלים שליליים. __בכל מקרה נוכל לזהות מעגלים שליליים לפי בלמן פורד שנשתמש בו כדי בכלל לחשב את כל המסלולים הקצרים ביותר לפי הגרף החדש__ .
כלומר ההנחה שלנו ששתי הגרפים לא מכילים מעגלים שליליים היא נכונה שכן אם אחד מהם מכיל אז גם השני ונוכל לצאת מהאלגוריתם.

לפי [[Computer Science/Algorithms/Single-Source Shortest Paths (SSSP)#תכונות של relaxation\|אי שיוויון המשולש]]  עבור קשת $(u,v)\in E^{\prime}$ יתקיים:
 $$\displaylines{
 \delta(s,v)\leq \delta(s,u)+w(u,v) \\ \downarrow \\
 0\leq  w(u,v)+\delta(s,u)-\delta(s,v) \\ \downarrow \\
 0\leq w(u,v)+ h(u)- h(v)\\ \downarrow \\ 
 0\leq \hat{w}(u,v)
 }$$

_שלב ראשון, הוספת הקודקוד s כקודקוד מקור_
![Pasted image 20230204221937.png|550](/img/user/Assets/Pasted%20image%2020230204221937.png)
_שלב שני, חישוב מחדש של המשקלים לפי $\hat{w}$_
![Pasted image 20230204222053.png|550](/img/user/Assets/Pasted%20image%2020230204222053.png)

__האלגוריתם__:
האלגוריתם של גונסון משתמש בבלמן פורד בשביל לבנות את המסלולים הקצרים ביותר על $G^{\prime}$ כאשר $s$ הוא קודקוד המקור ולאחר מכן מריץ דייקסטרה על כל קודקודי הגרף המקורי עם פונקציית משקל החדשה. 

``` psuedo
johnson(G=(V,E), w):
	initialize G'
	if bellman-ford(G',w ,s) == false 
		error : negative cycle
	for each u in G.V:
		h(u)= 𝜹(s,u)
	for each (u,v) in G.E:
		w'(u,v) = w(u,v) + h(u) - h(v)
	D = new matrix[n,n]
	for each v in V 
		dikstra(G, v, w')
		for each u in V
			\\the reverse function of w' to get the original w value
			d_uv = 𝜹'(u,v) - h(u) + h(v) 

	return D
```

זמן הריצה של האלגוריתם שקול ללהריץ פעם אחת בלמן פורד ו $|V|$ פעמים דייקסטרה כלומר:

$$O(|V||E|+|V|^{2}\log|V|)$$

