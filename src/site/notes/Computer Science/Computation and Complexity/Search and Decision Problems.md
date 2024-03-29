---
{"dateCreated":"2023-03-18 17:19","tags":["computational_complexity","computational_models","computer_science"],"pageDirection":"rtl","dg-publish":true,"permalink":"/computer-science/computation-and-complexity/search-and-decision-problems/","dgPassFrontmatter":true}
---


# בעיות חיפוש והכרעה
תורת החישוביות והסיבוכיות עונה על השאלה - ״איזה בעיות ניתן לפתור בצורה יעילה באמצעות מחשב״.
נזכר שבאמצעות __המודל החישובי__ ענינו על השאלה ״איזה בעיות ניתן לפתור באמצעות מחשב וראינו שיש קשר בין שפות שמחשב יכול לפתור לבין שפות כריעות שאלו שפות ש[[Computer Science/Computational Models/Turing Machine\|מכונת טיורינג]] יכולה לקבל. 

נתעסק בשתי סוגי בעיות 

1) בעיות חיפוש - מבקשות פתרון כלשהו (יכול להיות כמה פתרונות תקינים אבל צריך להחזיר רק אחד)
2) בעיות הכרעה - מספקות פתרון חד משמעי - true/false

ונענה על השאלה איך ממדלים אותם ומה הקשר בינהם.

## בעיות קנוניות
בעיות חיפוש מפורסמות שילוו אותנו כאשר נרצה למדל בעיות חיפוש נוספות.

### 3-COL
נתון גרף $G=(V,E)$ ונרצה למצוא __צביעה__ של קודקודי $G$ בשלושה צבעים ללא קשת ״מונוכרומטית״ כלומר לא קיימת קשת ששני הקצוות שלה צבועים באותו הצבע. 

![Pasted image 20230318173512.png|300](/img/user/Assets/Pasted%20image%2020230318173512.png)
למשל זה גרף שלא ניתן לצבוע כל קשת באופן הנ״ל

### 3-SAT
נתונה נוסחה בוליאנית בצורה $3CNF$ נסמנה $\rho$ והמכילה $\{X_{i}\}$ משתנים בוליאנים. נרצה למצוא השמה למשתנים שנותנת השמה מספקת כלומר המחזירה $true$
נזכיר שנוסחה בצורה $3CNF$ היא נוסחה מהצורה 

$$(X_{1}\vee X_{2}\vee X_{3})\wedge (X_{4}\vee X_{5}\vee X_{6})\wedge \dots$$

כלומר נוסחה שעוטפת 3 משתנים בוליאניים בפסוקיות או וכל קבוצה של 3 או פחות מחוברת לקבוצה אחרת עם וגם. (נשים לב ש $X_{i}$ יכול להיות גם מהצורה $\overline{X_{i}}$ כלומר המשלים)

נשים לב שאם נסמן פסוקית בודדת שמכילה 3 משתנים $\rho_{i}$ אז נוכל למצוא פסוקית שלא ניתן לספק אותה על ידי 

$$\rho_{i}\overline{\rho_{i}}$$

### מציאת מסלול בגרף 
עבור גרף $G=(V,E)$ וזוג קודקודים $u,v\in E$ נרצה לדעת האם קיים בינהם מסלול.

## Independent Set 
בהינתן גרף לא מכוון ומספר $k$ נרצה לבדוק האם יש בגרף $k$ קודקודים ללא אף קשת בינהם (קבוצה בלתי תלויה)

![Pasted image 20230325151424.png|300](/img/user/Assets/Pasted%20image%2020230325151424.png)

## מידול בעיות חיפוש
נתאר בעית חיפוש על ידי __יחסים__
יחס הוא אוסף של זוגות 

$$R=\{(x,y) \ \ | \ \ (x,y)\in\mathbb{R}\}$$

כאשר 

$$R\subseteq \{0,1\}^{*}\times \{0,1\}^{*}$$

ו $x$ מייצג בעיה כלשהי ו$y$ הוא הפתרון עבורה. 

למשל עבור בעיית ה 3col 

$$R_{3col}= \{(G,I)  \}$$

$I$ היא צביעה חוקית של $G$


__סימון:__ $R(x)$ יהווה את קבוצת אוסף הפתרונות של $x$ קלט כלשהו כלומר 

$$R(x)=\{y \ \  |  \ \ (x,y) \in \mathbb{R}\}$$

## אלגוריתם לפתרון בעיית חיפוש
אלגוריתם הוא אוסף כללים סופי שמגדיר תהליך חישובי במודל חישובי ״סביר״. כאשר סביר שקול למכונת טיורינג עם סרט בודד.

כלומר לפתור בעיית חיפוש זה כמו להגיד שלכל $x\in\{0,1\}^{*}$ 
* אם $R(x)\neq\emptyset$  אז $A(x)\in R(x)$ 
* אם $R(x)=\emptyset$ אז נסמן $A(x)=\perp$ סימון שמעיד שאין פתרון.


## בעיות הכרעה
בעיות שהתשובה עליהן היא true/false ואותם ממדלים על ידי קבוצות. 

$$\displaylines{
S\subseteq \{0,1\}^{*} \\
S=\{x \ \ | \ \ \text{x has a certain property}\}
}$$

כלומר קבוצת כל הקלטים שמחזירים true עבור הבעיה שלנו. 

למשל 

$$S_{3col}=\{G | \  \text{ G has a legit coloring}\}$$

__אלגוריתם $A$ פותר בעית הכרעה $S$__:

$$A(x)=1\leftrightarrow x\in S$$

## סיבוכיות זמן 
עבור מכונת טיורינג $M$ בעלת סרט בודד נסמן $S_{M}(x)$ - מספר הצעדים ש $M$ רצה על קלט $x$ עד שעוצרת (אם לא עוצרת אז $S_{M}(x)=\infty$) . 
כדי למדוד יעילות וסיבוכיות נגדיר עבור אלגוריתם $M$ 

$$T_{M}:\mathbb{N}\to\mathbb{N}$$

פונקציית זמן הריצה של $M$ שמקיימת

$$T_{M}(n) =\max_{|x|=n}\{S_{M}(x)\}$$

כלומר הזמן המקסימלי הדרוש כדי להריץ את את האלגוריתם $M$ על קלט באורך $n$.

__מה ייחשב יעיל?__
״פולינומי״ - החישוב של $M$ יקרא ״יעיל״ אם קיים פולינום $P(.)$ כלשהו כך שלכל $n$ מתקיים 

$$T_{M}(n)\leq P(n)$$

מצב זה נקרא __סיבוכיות זמן פולינומית__

אלגוריתם לא יהיה יעיל אם לא קיים פולינום כזה למשל עבור __סיבוכיות מעריכית כמו $2^{n}$__ .

>[!info] הבחנה
>אלגוריתם יעיל הוא אלגוריתם שרץ בזמן פולינומי __לאורך הקלט__ שלו למשל אם הקלט הוא $x,y$ אז האלגוריתם צריך לרוץ בזמן $P(x+y)$

## מחלקות של בעיות חיפוש
נרצה בתוך עולם של בעיות החיפוש למצוא את קבוצת בעיות החיפוש שיש להן אלגוריתם יעיל שמוצא פתרון. נסמן את קבוצה זאת כ $PF$ - polynomial find. יתקיים עבורו ש 

$R\in PF$ (R זאת בעיית חיפוש) אם:
* $R$ __חסום פולינומית__ כלומר  

$$\forall_{(x,y)\in{R}} :|y|\leq P_{R}(|x|)$$

כלומר אורך הפולט הוא פולינומי לאורך הקלט.
* קיים אלגוריתם פולינומי $A$ שפותר את $R$

>[!note] הבחנה
זה תנאי הכרחי אבל לא מספיק כלומר לא כל יחס שהפתרונות שלו הם קצרים יהיה ב $PF$ למשל $R_{3col}$ חסום פולינומית אבל לא ידוע אם הוא ב $PF$ כי לא מכירים אלגוריתם שפותר אותה ביעילות. מה שכן אנחנו יכולים לעשות זה לוודא שפתרון כלשהו הוא תקין כלומר בהינתן שיש פתרון נוכל לוודא אותו ביעילות.

__מחלקה חשובה נוספת של בעיות חיפוש__ נקראת $PC$ שמתארת בעיות _שוידוא פתרון_ ניתן לעשות ביעילות. polynomial check. במקרה זה, $R_{3col}\in PC$ . באופן פורמלי יותר $R\in PC$ אם $R$ חסום פולינומית וקיים אלגוריתם פולינומי $A$ כך שלכל זוג איברים $(x,y)$ יתקיים $A(x,y)=1\leftrightarrow (x,y)\in R$.

__שאלה:__ האם $PF\subseteq PC$ ?
אינטואיטיבית נראה שכן אבל בפועל זה לא המצב.
אם כן , ישנו $R\in PF$ ושמקיים $R\notin PC$ ולכן אין הכלה. ההסבר ללמה זה נכון היא שבאלגוריתם וידוא פתרון לפעמים לאמת פתרון קשה מאוד יהיה יותר מורכב מלמצוא אותו. לדוגמה :

$$\displaylines{R=\{(\langle m,x\rangle , 0) \ \ | \ \ x\in \{0,1\}^{*} \ \ , \ \ \text{m is turing machine}\}  \\ \cup \\ \{(\langle m,x\rangle , 1) \ \ | \ \ x\in \{0,1\}^{*} \ \ , \ \ \text{m is turing machine that accept x}\} } $$


$R\in PF$ עבור $\langle m,x\rangle$ תמיד $0$ היא תשובה נכונה כי זה איחוד של כל הקלטים עם 0 וקלטים תקינים עם 1. 

$R\notin PC$ היא בעצם בעיה הדורשת מאיתנו לבדוק האם $m$ עוצרת על $x$ שזאת בעיה לא כריעה. 

>[!info] הבחנה
>לא ידוע האם $PC\subseteq PF$ 

## מחלקות של בעיות הכרעה
מהן המקבילות של $PC,PF$ בעולם של בעיות הכרעה?

* המקבילה ל $PF$ נקראת $P$ והיא מייצגת את אוסף בעיות ההכרעה שיש להן אלגוריתם פולינומי כלומר קיים להן אלגוריתם מכריע $A$. למשל- $S_{2col}\in P$  
* המקבילה של $PC$ היא קבוצת אלגוריתמים שכל אחד מהם הוא __מוודא__ או באופן פורמלי אלגוריתם $V$ עבור בעיית הכרעה $S$ כלשהי שמקיים 2 תנאים 
	* _אי שלמות_- לכל $x\in S$ קיים $y$ כך ש $V(x,y)=1$ 
	* _נאותות_- אם $x\notin S$ אזי לכל $y$ מתקיים $V(x,y)=0$ 

כלומר 

$$V=\begin{cases}
\exists_{y\in\{0,1\}^{*}}: V(x,y)=1 \leftrightarrow x\in S \\ \\
\forall_{y\in\{0,1\}^{*}}: V(x,y)=0 \leftrightarrow x\notin S 
\end{cases}$$

סך הכל, נגדיר מחלקה של בעיות הכרעה שניתנות לווידוא כ $NP$ ויתקיים $S\in NP$ אם קיים ל $S$ מוודא פולינומי מסוג $NP$ :
כלומר אם קיים מוודא $V_{S}(\cdot)$ ופולינום $P_{S}(\cdot)$ כך ש

$$\displaylines{
x\in S \to \exists_{|y|\leq P_{S}(|x|)}: V_{S}(x,y)=1\\
x\notin S \to \forall_{|y|\leq P_{S}(|x|)}: V_{S}(x,y)=0
}$$

ו $V_{S}$ הינו אלגוריתם שזמן ריצתו הוא פולינומי ב $|x|$ .

>[!note] נשים לב
>אומנם אמרנו ש $PC$ היא המקבילה של $NP$ ומתקיים  $PF\not\subset PC$  אבל יתקיים ש $P\subseteq NP$ בגלל ה __עד הריק__, לכל $S\in P$ יש אלגוריתם פולינומי $A_{S}$ שמכריע את $S$ ויתקיים ש $S$ כזו היא ב $NP$ כי נוכל להגדיר מוודא פולינומי באופן הבא $V_{S}(x,y)= A_{S}(x)$ כלומר הוא מתעלם מהעד ומכריע רק לפי הקלט.

__משפט:__ לכל בעית חיפוש $R$ ניתן להגדיר בעית הכרעה מתאימה $S_{R}=\{\ x \ | \ \exists_{y}:(x,y)\in R \}$ ובפרט מתקיים $R\in PF\to S_{R}\in P$ .

## בעיית קליקה
יהי $G=(V,E)$ גרף לא מכוון.
קבוצת קודקודים $S\subseteq V$ נקראת ״קליקה״ אם לכל $u,v\in S$ יש קשת בין $u$ ל $v$ 

![Pasted image 20230319193616.png|300](/img/user/Assets/Pasted%20image%2020230319193616.png)

נגדיר את בעיית החיפוש הבאה 

$$R_{4Clique}=\{(G,S) \ |  \text{S is a clique with size 4 in G } \}$$

נרצה להראות ש $R_{4C}\in  PF$ . כלומר לבנות אלגוריתם שמוצא פתרון בזמן פולינומי לגודל של $S$ . ראשית נשים לב שהיחס עצמו חסום פולינומית כי לכל $(G,S)$ ביחס מתקיים $|S|\leq |G|$ . כעת נוכל לבנות את האלגוריתם 

``` psuedo
A(G):
	create S with 4 vertex
	for all u,v in S 
		if (u,v) not in E
			return NULL
		return S 
```

זה האלגוריתם הנאיבי ביותר ולכן ברור שהוא פותר את הבעיה. הוא גם פולינומי כי מספר תתי הקבוצות בגודל 4 הוא $\binom{|V|}{4}\leq |V|^{4}$.

__נרחיב את הבעיה לבעיה כללית יותר__ 

$$R_{kClique}=\{((G,k),S) \ |  \text{S is a clique with size at lest k in G } \}$$

כעת נרצה לבדוק האם היחס הזה גם שייך ל $PF$ . במקרה הזה מתקיים שהאלגוריתם מהתרגיל הקודם אינו פולינומי כי נקבל שנרצה לבנות את תתי הקבוצות בגודל $k$ הינו 

$$\binom{|V|}{k}\geq \left(\frac{|V|}{k}\right)^{k}$$

שבהתאם ל $k$ יכול להיות אקספוננציאלי לגודל הקלט למשל אם $k= \frac{|V|}{2}$ נקבל שזמן הריצה יהיה גדול או שווה ל $2^{\frac{|V|}{2}}$ . 

$$\left(\frac{|V|}{\frac{|V|}{2}}\right)^{\frac{|V|}{2}}= 2^{\frac{|V|}{2}}$$

אם כן התשובה לשאלה נשארת פתוחה כי אנחנו לא יודעים אם קיים אלגוריתם ומצד שני לא קיבלנו עדיין כלים לקבוע באופן חד משמעי בלי למצוא אלגוריתם.

האם $R_{kC}\in PC$ ? __כן__
בהינתן $((G,k), S)$ האלגוריתם יבדוק אם $|S|\geq k$ ושלכל $u,v\in S$ מתקיים $\{u,v\}\in E$ אם הבדיקות עברו בהצלחה יחזיר $1$ אחרת $0$. כעת יותר קל לאמת כי אנחנו מביאים איזשהו ״עד״ כמו שאמרנו. זמן הריצה יהיה חסום על ידי 

$$\binom{|S|}{2}\leq |S|^{2}\leq |G|^{2}$$

## בעיית composite 
נגדיר את בעיית ההכרעה הבאה 

$$\text{composite}=\{n\in\mathbb{N} \ | \ \text{n is not prime}\}$$

נרצה להראות שהבעייה שייכת ל $NP$ . אכן נגדיר 

``` psuedo 
V(n,k)
	if 1<k<n AND n mod k == 0 
		return 1
	return 0
```

האלגוריתם אכן מקיים אי שלמות ונאותות עבור composite :

$$\forall_{n\in composite}\exists_{2\leq k\leq n-1}: V(n,k)=1$$

אחרת אם הוא לא שייך בוודאי שאין לו מחלק בתחום הזה כי הוא ראשוני

והוא בהחלט פולינומי כיוון שגודל ה״עד״ חסום בגודל הקלט. 

>[!info] סיכום: סוגי מחלקות
>__חיפוש :__
>	1) ==מציאה== $PF$ קבוצה של יחסים $R$ חסומים פולינומית כך שלכל $R$ קיים אלגוריתם פולינומי שמוצא פתרון כלשהו
>	2) ==ווידוא== $PC$ קבוצה של יחסים $R$ חסומים פולינומית כך שלכל $R$ קיים אלגוריתם פולינומי שמקבל $(x,y)$ ומוודא שהוא אכן שייך ל $R$ 
>__הכרעה :__
>	1) ==מציאה== $P$ קבוצה של $S$ בעיות כך שלכל בעיה קיים אלגוריתם פולינומי שמכריע אותה כלומר האם איבר שייך לקבוצה או לא.
>	2) ==ווידוא== $NP$ קבוצה של $S$ כך שלכל בעיה קיים מוודא פולינומי $V_{S}$ 

## הכלה של המחלקות
הראנו אם כן ש $PF\not\subseteq PC$ וש $P\subseteq NP$ . 
מה לגבי: $PC\subseteq PF$ ו $NP\subseteq P$ ?

אם כן ההכלה של אחד בשני אינה כה ברורה אבל נוכל לטעון את הטענה הבעיה 

$$PC\subseteq PF\leftrightarrow NP\subseteq P$$

__הוכחה:__
$\leftarrow$ : נניח $PC\subseteq PF$ ונרצה להראות ש $NP\subseteq P$ .
תהי $S\in NP$ כלומר קיים $V_{S}$ פולינומי כך ש 

$$x\in S \leftrightarrow \exists_{ y : |y|\leq P(|x|)}: V_{S}(x,y)=1$$

נגדיר 

$$R_{S}=\{(x,y)  \ \ | \ \  V_{S}(x,y) =1\}$$

$R_{S}$ אכן חסום פולינומית כי מוגדר לפי הזוגות שהמוודא מקבל ולפי הגדרה $y$ חסום פולינומית ל $x$ המתאים לו. כמו כן $V_{S}$ הוא אלגוריתם פולינומי ולכן $R_{S}\in PC$ .

מההנחה ש $PC\subseteq PF$ מתקיים $R_{S}\in PF$ ולכן קיים אלגוריתם פולינומי $A_{R_{S}}$ שמקיים 

$$A_{R_{S}}(x)\neq \perp\leftrightarrow R_{S}(x)\neq \emptyset\leftrightarrow \exists_{ y : |y|\leq P(|x|)}: V_{S}(x,y)=1\leftrightarrow x\in S $$

לכן נוכל להגדיר $A_{S}(x)$ אלגוריתם פולינומי ל $S$ על ידי 

$$A_{S}(x)= \begin{cases}
1 & A_{R_{S}}(x)\neq\perp   \\ \\
0 & A_{R_{S}}(x)=\perp  
\end{cases}$$

ומתקיים ש $S\in P$ _כדרוש_.


$\rightarrow$ : נניח $NP\subseteq P$ וצריך להראות ש $PC\subseteq PF$.
יהי $R\in PC$ נרצה להראות שהוא גם ב $PF$ 
אם כן, עבור $R$ מתקיים שישנו אלגוריתם פולינומי $A_{R}$ כך ש 

$$(x,y)\in R \leftrightarrow A_{R}(x,y)=1 $$

אם כן, נגדיר 

$$S_{R}=\{x \ \ | \ \ \exists_{y:|y|\leq P(|x|)}: (x,y) \in R\}$$

מתקיים $S_{R}\in NP$ כי אפשר להשתמש ב $A_{R}$ בתור המוודא הפולינומי ל $S_{R}$  כי מהגדרתו נובע ש $x\in S_{R}$ אמ״מ $A_{R}(x,y)=1$ כאשר קיים $y$ שכזה חסום פולינומי באורכו של $x$.

מההנחה מתקיים ש $S_{R}\in P$ כלומר קיים $A_{S_{R}}$ אלגוריתם שמכריע את $S_{R}$ 

$$A_{S_{R}}(x)=1\leftrightarrow x\in S_{R}$$

כעת , נגדיר בעיה שקולה ל $S_{R}$ :

$$S^{\prime}_{R} = \{[x,y^{o}] \ | \ \exists_{y^{\prime}}: (x,y^{o}\circ y^{\prime}) \in R\}$$

כלומר בעיה שמכילה את הקלט ביחד עם כל התחיליות של הפתרון שלו בבעיית החיפוש $R$ . גם כאן מתקיים $S^{\prime}_{R}\in NP$ כיוון שנוכל להשתמש בוריאציה של $A_{R}$ כמוודא פולינומי כלומר 

$$[x,y]\in S^{\prime}_{R}\leftrightarrow \exists_{y^{\prime}}: A(x,y\circ y^{\prime})=1$$

זה עדיין פולינומי כי מציאת $y^{\prime}$ שכזה היא עדיין פולינומית ולכן $S^{\prime}_{R}\in NP$ , מההנחה מתקיים גם ש $S^{\prime}_{R}\in P$ ולכן קיים לו אלגוריתם $A_{S^{\prime}_{R}}$ שמכריע כל קלט מהצורה $[x,y]$ .

__אלגוריתם פולינומי למציאת פתרון של $R$__ (קלט: $x$)
- בדוק האם $x\in S_{R}$ 
	- אם התשובה היא `false` החזר $\perp$ .
* אחרת, הגדר `'' = y` .
* כל עוד $(x,y)\notin R$ (ניתן לוודא באמצעות $A_{R}$) 
	* בדוק האם  $[x,y\circ 0]\in S^{\prime}_{R}$
	* אם כן, $y=y\circ 0$
	* אחרת  $y=y\circ 1$
 * החזר $y$

__כדרוש__.