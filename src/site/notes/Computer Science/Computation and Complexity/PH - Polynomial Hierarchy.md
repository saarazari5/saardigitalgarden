---
{"dateCreated":"2023-06-30 19:26","tags":["computational_complexity","computational_models","computer_science"],"pageDirection":"rtl","dg-publish":true,"permalink":"/computer-science/computation-and-complexity/ph-polynomial-hierarchy/","dgPassFrontmatter":true}
---


# היררכיה פולינומית

נתבונן ב 3 הבעיות הבאות : 
1. $clique$ - $\{(G,k) \ | \ \text{G has k vertex with all the possible edges}\}$
2. $\overline{clique}$ - $\{(G,k) \ | \ \text{G does not contains a k-size clique}\}$
3. $maxClique$ - $\{(G,k) \ | \text{k is the maximal size clique in G}\}$ 

הראשונה שייכת ל [[Computer Science/Computation and Complexity/Search and Decision Problems\|NP]] שכן העדות יכולה להיות פשוט $k$ קודקודים מהגרף.
השנייה כמובן שייכת ל [[Computer Science/Computation and Complexity/CO-P and CO-NP#CO-NP\|CONP]].  
לאן שייכת $maxClique$ ? 
כדי ש $(G,k)$ יהיה שייך ל $maxClique$ צריך להראות ש 
1. ב $G$ __קיים__ קליק בגודל $k$.
2. __לכל__ קבוצת קודקודים ב $G$ בגודל העולה על $k$, קבוצה זאת אינה קליק.

כלומר את הבעיה הזאת אנחנו מתארים באמצעות שתי כמתים, בניגוד לבעיות $NP,CONP$ שאפשר לתאר אותן עם כמת אחד בלבד.

אם כן $maxClique\in\Sigma_{2}$ שאותו נגדיר באמצעות ההירכייה הפולינומית:

![Pasted image 20230630194359.png](/img/user/Assets/Pasted%20image%2020230630194359.png)

__הגדרה__ נאמר כי $S\in\Sigma_{k}$ אמ״מ קיים מוודא פולינומי כך ש 

$$x\in S\leftrightarrow \exists_{y_{1}}\forall_{y_{2}}\exists_{y_{3}}\forall_{y_{4}}\dots Q_{y_{k}}: V_{S}(x,y_{1},y_{2},\dots, y_{k})=1 $$

כאשר: $Q_{}=\exists$  אם $k$ אי זוגי ו $Q_{}=\forall$ אחרת.
וגם קיים פולינום $P$ כך ש 

$$\forall_{1\leq i\leq k}: |y_{i}|\leq P(|x|)$$

>[!info] הבחנה
>$\Sigma_{0}=P$ כי אין עדים כלל, רק אלג׳ פולינומי.
>$\Sigma_{1}=NP$ כי יש עדות אחת


__למשל__: $S\in\Sigma_{2}$ אם קיים פולינום $P$ ואלגוריתם פולינומי $V$ כך ש 

$$x\in S\leftrightarrow \exists_{y_{1}}\forall_{y_{2}}: V(x,y_{1},y_{2}) = 1$$

>[!note] הבחנה
>$NP\subseteq \Sigma_{2}$ כי אפשר להתעלם מ $y_{2}$ 
>$CONP\subseteq\Sigma_{2}$ כי אפשר להתעלם מ $y_{1}$ 
>
![Screenshot 2023-07-01 at 17.51.43.png|400](/img/user/Assets/Screenshot%202023-07-01%20at%2017.51.43.png)

__טענה__:
$$\forall_{k\in\mathbb{N}}: \Sigma_{k}\subseteq \Sigma_{k+1}$$

לא אתעמק בהוכחה בסיכום זה אך ניתן להראות באינדוקצייה שאם $s\in\Sigma_{i}$ אז המוודא החדש יוכל לקבל $i+1$ עדים ולהשתמש במודוא המקורי תוך התעלמות מהעדות האחרונה כך ש $s\in\Sigma_{i+1}$ .

__הגדרה__ נאמר כי $S\in\Pi_{k}$  אמ״מ קיים מוודא $V_{S}$ כך ש 

$$x\in S\leftrightarrow \forall_{y_{1}}\exists_{y_{2}}\forall_{y_{3}}\dots Q_{y_{k}}$$

כאשר: $Q_{}=\exists$  אם $k$ זוגי ו $Q_{}=\forall$ אחרת.
וגם קיים פולינום $P$ כך ש 

$$\forall_{1\leq i\leq k}: |y_{i}|\leq P(|x|)$$

>[!note] הבחנה
>$\Pi_{0}=P$ כי אין עדים כלל, רק אלג׳ פולינומי.
>$\Pi_{1}=CONP$ כי מתקבלת עדות של ״לכל״

__טענה__
תחת אותה הוכחה נוכל להראות ש 

$$\forall_{k\in\mathbb{N}}: \Pi_{k}\subseteq \Pi_{k+1}$$

__טענה__

$$\forall_{k\in\mathbb{N}}: \Sigma_{k}\subseteq\Pi_{k+1} \ \ , \ \ \Pi_{k}\subseteq \Sigma_{k+1}$$

__הגדרת ההירכייה הפולינומית__

$$PH=\bigcup_{k=0}^{\infty}\Sigma_{k}=\bigcup_{k=0}^{\infty}\Pi_{k}$$

>[!note] הבחנה
>$\Sigma_{k}= CO\Pi_{k}$ וגם הפוך ...

>[!question] שאלה 
>אם יש [[Computer Science/Computation and Complexity/Cook Reduction\|רדוקציית קוק]] מבעיה $A$ לבעיה $B\in NP$ אז לאן שייכת $A$?

## משפט קריסת ההירכייה

$$P=NP \leftrightarrow P=PH$$

או במילים אחרות 

$$\Sigma_{0} = \Sigma_{1}\leftrightarrow P=PH$$

כדי להוכיח את הטענה הזאת ולענות על השאלה שלנו נרצה לתת מספר למות עזר ואת הוכחותיהן

__למה 1__ :
$S\in\Sigma_{k+1}$ אמ״מ קיים פולינום $P$ ובעיה $S^{\prime}\in \Pi_{k}$ כך ש 

$$x\in S \leftrightarrow \exists_{y:|y|\leq P(|x|)}:(x,y)\in S^{\prime}$$

__הוכחת למה 1__: 
$\leftarrow$ :
תהי $S\in\Sigma_{k+1}$ , לפי הגדרת $\Sigma_{k+1}$ קיים מוודא $V_{S}$ ופולינום $P$ כך ש

$$x\in S \leftrightarrow \exists_{y_{1}}\forall_{y_{2}}\dots Q_{y_{k+1}}: V_{S}(x,y_{1},y_{2},\dots, y_{k+1})=1 $$

וגם 

$$\forall_{i} : |y_{i}|\leq P(|x|)$$

נגדיר את $S^{\prime}$ לקבל $(x,y_{1})$ כך ש 

$$(x,y_{1})\in S^{\prime}\leftrightarrow \forall_{y_{2}}\dots Q_{y_{k+1}}: V((x,y_{1}),y_{2},\dots, y_{k+1})= 1$$

נשים לב ש $S^{\prime}\in\Pi_{k}$ ועבור אותו פולינום $P$ שחוסם את $|y_{1}|$ תחת הבעיה $S$ יתקיים שהוא חוסם את $|y_{1}|$ תחת הבעיה $S^{\prime}$.


$\rightarrow$ :
אם קיים $S^{\prime}\in\Pi_{k}$ ופולינום $P$ המקיים $x\in S \leftrightarrow \exists_{y:|y|\leq P(|x|)}:(x,y)\in S^{\prime}$ אזי $S\in\Sigma_{k+1}$ שכן קיים מוודא $V_{S^{\prime}}$ פולינומי, ופולינום $P^{\prime}$ כך שלפי הגדרה : 

$$(x,y)\in S^{\prime}\leftrightarrow \forall_{y_{1}}\exists_{y_{2}}\dots Q_{y_{k}}: V_{S^{\prime}}:((x,y),y_{1},\dots,y_{k})=1$$

וגם $|y_{i}|\leq P^{\prime}(|x,y|)$

נשים לב אם כן שמהנתונים ומההגדרה מתקיים 

$$x\in S \leftrightarrow \exists_{y:|y|\leq P(|x|)}:\forall_{y_{1}}\exists_{y_{2}}\dots Q_{y_{k}}: V_{S^{\prime}}:((x,y),y_{1},\dots,y_{k})=1$$

כך ש $|y_{i}|\leq P^{\prime}(|x,y|)$ וגם $|y|< P(|x|)$

קיבלנו בעצם ביטוי מהצורה של $\Sigma_{k+1}$ , נוכל אם כן להגדיר אם כן 

$$V_{S}(x,y,y_{1},y_{2},\dots , y_{k})= V_{S^{\prime}}((x,y),y_{1},y_{2},\dots , y_{k})$$

והוא מוודא מתאים לקריטריון לכך ש $S\in\Sigma_{k+1}$ .

__המשמעות של הלמה היא שניתן לעבור מבעיה ב $\Sigma_{k+1}$ לבעיה ב $\Pi_{k}$ והפוך__

__למה 2__ :
עבור $k\geq 1$ אם $\Pi_{k}\subseteq\Sigma_{k}$ אזי $\Sigma_{k}=\Sigma_{k+1}$ .

>[!note] הבחנה
_עבור $k=0$ נקבל $P\subseteq P$ שזה כמובן נכון ולכן נקבל $P=NP$ שזו שאלה פתוחה ומניחים שזה לא מתקיים._ 

__הוכחת למה 2:__
נניח $\Pi_{k}\subseteq\Sigma_{k}$ ונרצה להראות $\Sigma_{k+1}\subseteq\Sigma_{k}$ . תהי $S\in\Sigma_{k+1}$ . לפי למת עזר 1 נובע כי קיימת $S^{\prime}\in\Pi_{k}$ ופולינום $P$ כך ש 

$$x\in S \leftrightarrow \exists_{y, |y|\leq P(|x|)}: (x,y)\in S^{\prime}$$

מההנחה שלנו ש $\Pi_{k}\subseteq\Sigma_{k}$ נקבל ש $S^{\prime}\in\Sigma_{k}$ כלומר :

$$(x,y)\in S^{\prime}\leftrightarrow \exists_{y_{1}}\forall_{y_{2}}\dots Q_{y_{k}}: V_{S^{\prime}}((x,y), y_{1},\dots, y_{k})$$

וכן $|y_{i}|\leq P^{\prime}(|x,y|)$.  סך הכל נקבל ש 

$$x\in S \leftrightarrow  \exists_{y, |y|\leq P(|x|)}: \exists_{y_{1}}\forall_{y_{2}}\dots Q_{y_{k}}: V_{S^{\prime}}((x,y), y_{1},\dots, y_{k}) $$

קיבלנו סדרה של שני כמתי קיים רצופים ולכן נוכל להגדיר מוודא $y_{0}=y\circ y_{1}$ ולאחד לכמת אחד ולקבל 

$$x\in S \leftrightarrow  \exists_{y_{0}}\forall_{y_{2}}\dots Q_{y_{k}}: V_{S^{\prime}}((x,y), y_{1},\dots, y_{k}) $$

כל שנשאר לעשות הוא להגדיר את המוודא של $S$ להיות

$$V_{S}(x,y_{0},y_{2},\dots ,y_{k})= V_{S^{\prime}}((x,y),y_{1},\dots,y_{k})$$

כלומר הוא סך הכל יקבל את הקלט בצורה שונה.
מכאן מתקיים ש $S\in\Sigma_{k}$ כנדרש.

__למה 3__ :

$$\forall_{k\geq 0}: \Sigma_{k}=\Sigma_{k+1}\to \Sigma_{k+1}=\Sigma_{k+2}$$

טענה זו גוררת באופן ישיר שאם $\Sigma_{0}=\Sigma_{1}$ אזי $P=NP$ וזה אומר שכל ההירכייה קורסת.

__הוכחת למה 3:__
נניח ש $\Sigma_{k}=\Sigma_{k+1}$ .
מלמה 2 נרצה להראות ש $\Pi_{k+1}\subseteq \Sigma_{k+1}$ ונקבל $\Sigma_{k+1}=\Sigma_{k+2}$ .
מהגדרת ההירכייה כבר יש לנו ש $\Pi_{k}\subseteq \Sigma_{k+1}$ .
מההנחה שלנו מתקיים ש 

$$\Pi_{k+1}=\Pi_{k}$$ 
כי מתקיים $\Pi_{k}=\overline{\Sigma_{k}}$ לכל $k$. 

אם כן סך הכל נקבל 

$$\Pi_{k+1}\subseteq \Sigma_{k+1}$$

כי $\Pi_{k+1}=\Pi_{k}\subseteq\Sigma_{k+1}$ ומטענת העזר הקודמת נקבל $\Sigma_{k+1}=\Sigma_{k+2}$ .

__את משפט קריסת ההירכיה כעת אפשר להוכיח ישירות באמצעות למה 3__ 
אם $P=NP$ אזי $\Sigma_{0}=\Sigma_{1}$ ולכן מלמה 3 השיוויון חל לכל $k$ . כלומר 

$$PH=\bigcup_{i=0}^{\infty}\Sigma_{k}=\Sigma_{0}= P$$

>[!info] מסקנה
>אם $\Sigma_{k}=\Sigma_{k+1}$ אז $PH=\Sigma_{k}$ כי לכל $i\leq k$ מתקיים $\Sigma_{i}\subseteq \Sigma_{k}$ וכל $j\geq k$ מקיים שיוויון.

כעת נוכל להתחיל לענות על השאלה ששאלנו למעלה , איך ניתן להסיק שאם יש רדוקציית קוק מ $A$ ל $B\in\Sigma_{k}$ אז $A\in PH$? 

נרצה להסתכל על מכונות טיורינג __לא דטרמניסטיות__ בעלת גישת אורקל ל $B$.

1) נסמן: $NP^{B}$ מחלקת הבעיות שניתן לפתור על ידי מכונת טיורינג פולינומית __לא דטרמינסטית__ בעלת גישת אורקל ל $B$.
2) נסמן: $P^{B}$ מחלקת הבעיות שניתן לפתור על ידי מכונת טיורינג פולינומית __דטרמינסטית__ בעלת גישת אורקל ל $B$.
3) נסמן: $P^{NP}$ מחלקת הבעיות שניתן לפתור על ידי מכונת טיורינג פולינומית __דטרמינסטית__ בעלת גישת אורקל ל $B\in NP$. 
4) נסמן: $NP^{NP}$ מחלקת הבעיות שניתן לפתור על ידי מכונת טיורינג פולינומית __לא דטרמינסטית__ בעלת גישת אורקל ל $B\in NP$. 

__משפט__: אם יש רדוקציית קוק מ $A$ ל $B\in NP$ אזי: $A\in P^{NP}\subseteq NP^{NP}=NP^{\Sigma_{1}}$ .

## מכונת טיורינג לא דטרמינסטית
[[Computer Science/Computational Models/Turing Machine#מודל לא דטרמינסטי\|מכונת טיורינג לא דטרמינסטית]] היא מ״ט שמסלול החישוב שלה על הקלט לא מוגדר ביחידות וייתכנו מספר מסלולי חישוב אפשריים על אותו קלט.
נאמר כי מכונה כזו מקבלת קלט אם יש לה מסלול חישוב על הקלט שמגיע למצב מקבל.
במצב אחר, נאמר כי היא אינה מקבלת את הקלט (מגיעה למצב של דחייה או לא עוצרת).

מ״ט ל״ד יכולה לקיים את התכונות הבאות:
1) פולינומית - מספר הצעדים __בכל__ מסלולי החישוב הוא פולינומי בגודל הקלט
2) בעל גישת אורקל- יכולה להפעיל שאילתות לבעיות ההכרעה ב$NP$ בעלות של צעד בודד.

__משפט__: 
לכל $k\geq 0$ מתקיים 

$$NP^{\Sigma_{k}}=\Sigma_{k+1}$$

נוכיח בהכלה דו כיוונית-
$\supseteq$ : 
תהי $S\in\Sigma_{k+1}$ , נוכיח ש $S\in NP^{\Sigma_{k}}$ .
נשים לב ראשית לתכונה הבאה 

$$NP^{\Sigma_{k}}=NP^{\Pi_{k}}$$

זה נובע בגלל ש $\Pi_{k}= CO\Sigma_{k}$ כלומר, כיוון שהתשובות של האורקל נכונות תמיד, לענות על השאלה האם $x\in S^{\prime}\in\Sigma_{k}$ זה שקול ללענות על השאלה האם $x\in \overline{S^{\prime}}\in \Pi_{k}$.

כמו כן נזכר בלמה שאומרת 

$$S\in\Sigma_{k+1}\leftrightarrow\exists_{P(),S^{\prime}\in\Pi_{k}}:(x\in S\leftrightarrow \exists_{y,|y|\leq P(|x|)}: (x,y)\in S^{\prime})$$

נראה שאם $S\in\Sigma_{k+1}$ אז $S\in NP^{\Sigma_{k}}=NP^{\Pi_{k}}$

עבור $S$ כנ״ל נבנה מ״ט פולינומית ל״ד שמכריעה את $S$ על ידי גישת אורקל ל $S^{\prime}\in\Pi_{k}$ שהיא הבעיה המובטחת מהלמה הנ״ל.

$M_{S}(x)$ :
1. הגרל $|y|\leq P(|x|)$ (הפולינום המתאים מהלמה)
2. פנייה לאורקל על $S^{\prime}$ ושאלה האם $(x,y)\in S^{\prime}$ והחזרת התשובה.

מהלמה נובע כי אם $x\in S$ אזי קיים $y$ שיגרום למכונה $M_{S}$ לקבל את $x$ כי ישנו איזה  $y$ שעבורו $(x,y)\in S^{\prime}$. 
אחרת, אם $x\notin S$ אזי לכל $y$, $(x,y)\notin S^{\prime}$ ולכן $M_{S}$ דוחה כלומר ל $M_S$ אין מסלול.

ז״א ש $S\in NP^{\Pi_{k}}=NP^{\Sigma_{k}}$ כדרוש.

$\subseteq$ :
נניח כי $S\in NP^{\Sigma_{k}}$ ונוכיח כי $S\in\Sigma_{k+1}$ 
בלי הגבלת הכלליות נניח שהמכונה טיורינג ל״ד שמכריע בקריאות לאורקל היא מכונה כזו שמגרילה תשובות בלי קריאה לאורקל ורק אם כל ההגרלות מתקבלות על ידי המכונה, האורקל בודק את כל ההגרלות ויחזיר $1$ אם כל ההגרלות נכונות גם מבחינת האורקל. 
המטרה היא להפריד את ה״לוגיקה״ של המכונה לבין הפניות לאורקל.

![Pasted image 20230701202914.png](/img/user/Assets/Pasted%20image%2020230701202914.png)

אם כן , נניח ש $M_{S}$ מחולקת ל 2 חלקים :
_ביצועי-_ בו לא פונים לאורקל אלא מנחשים תשובות ועובדים איתן עד שמגיעים להכרעה.
_שאילתות-_ בחלק זה מתבצעות בצורה מרוכזת כל הפניות לאורקל על ההגרלות שבוצעו ורק אם אין בעיית תאימות יש לקבל את הקלט.

עבור קלט $x$ והגרלות $y$ של המכונה הלא דטרמנסטית, 
נסמן ב $q(x,y)$ את מספר השאלות שרוצים לשאול באורקל.
נסמן ב $q^{i}(x,y)$ את השאלה ה $i$ מבין $q(x,y)$ שאלות (שאילתה אל מול האורקל)
נסמן ב $a^{i}(x,y)$ את הניחוש ה $i$

מתקיים ש 

$$x\in S\leftrightarrow \exists_{y:|y|\leq |P(|x|)|}(A(x,y)=1)\wedge \bigg(\bigwedge_{i=1}^{q(x,y)} (\underbrace{(a^{i}(x,y)=1)}_{E_{1}^{i}}\leftrightarrow \underbrace{(q^{i}(x,y)=1))}_{E_{2}^{i}}\bigg)$$

כאשר $A$ זה החלק הביצועי של המכונה בלי פניות לאורקל.
נזכיר שנרצה להראות ש $x\in S$ אם הוא ביטוי מהצורה של $\Sigma_{k}$ . 

נשים לב ש $E_{1}\leftrightarrow E_{2}$ זה שקול לוגית ל $(E_{1}^{i}\wedge E_{2}^{i})\vee(\urcorner{E_{1}^{i}}\wedge \urcorner{E_{2}^{i}})$

נציב ונקבל :

$$((a^{i}(x,y)=1)\wedge (q^{i}(x,y)=1))\vee ((a^{i}(x,y)=0)\wedge (q^{i}(x,y)=0))$$

כעת נסתכל על הבעיה $S^{\prime}\in\Sigma_{k}$ הבעיה שאליה נעשת הפניית אורקל. 
לבעיה זא קיים מוודא פולינומי $V_{S^{\prime}}$ ופולינום $P_{S^{\prime}}$ כך ש:

$$w\in S^{\prime}\leftrightarrow \exists_{y_{1}}\forall_{y_{2}}\dots Q_{y_{k}}: V_{S^{\prime}}(w,y_{1},\dots, y_{k})=1$$

באותו אופן :

$$w\notin S^{\prime}\leftrightarrow\urcorner(\text{""})=\forall_{y_{1}}\exists_{y_{2}}\dots Q_{y_{k}}: V_{S^{\prime}}(w,y_{1},\dots,y_{k})=0 $$

הדבר נכון גם עבור השאילתות ה $q^{i}$ שרשמנו למעלה. בסופן של דבר נוכל להציב את הנ״ל בביטוי $(E_{1}^{i}\wedge E_{2}^{i})\vee(\urcorner{E_{1}^{i}}\wedge \urcorner{E_{2}^{i}})$ עד שנקבל את הדרוש כלומר $S\in\Sigma_{k+1}$. 

אם כן , הוכחנו ש $\Sigma_{k+1}= NP^{\Sigma_{k}}$ .

## סגירות רדוקציית קוק
תהי $A$ כלשהי ו $B\in\Sigma_{k}$ אזי, 
אם ישנה רדוקציית קוק מ $A$ ל $B$ כלומר ישנה מ״ט פולינומית דטרמיניסטית בעלת גישת אורקל ל $B$ שמכריעה את $A$ ונסמן אותה $A\in P^{B}$ 

ברור כי $P^{B}\subseteq NP^{B}$ כי המודל הדטרמיניסטי מוכל במודל הלא דטרמיניסטי של מ״ט.
עבור $B\in\Sigma_{k}$ מקבלים ש $A\in NP^{\Sigma_{k}}$ ומהטענה הנ״ל מתקיים $A\in\Sigma_{k+1}$ כלומר ישנה סגירות לרדוקציית קוק תחת $PH$ .

>[!note] מסקנה
>אם $NP=CONP$ אזי $NP=NP^{NP}$  כי אם $\Sigma_{1}=\Pi_{1}$ אזי $\Sigma_{1}=\Sigma_{2}=NP^{NP}$ 




