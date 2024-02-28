---
{"dateCreated":"2023-06-02 20:39","tags":["computational_complexity","computational_models","computer_science"],"pageDirection":"rtl","dg-publish":true,"permalink":"/computer-science/computation-and-complexity/co-p-and-co-np/","dgPassFrontmatter":true}
---



# CO-P ו CO-NP

## CO-NP
נגדיר את $CO-NP$ להיות :

$$CO-NP=\{S \ |  \ \overline{S}\in NP\}$$

דוגמה: נתבונן בבעיה 

$$\overline{SAT}= \{\rho \ |\ \rho \text{ cannot be satisfied} \}$$

כיוון ש $SAT\in NP$ מתקיים ש $\overline{SAT}\in CO-NP$ .

==השערה-== $CO-NP\neq NP$ 
==טענה-==  $P\subseteq CO-NP$ . הוכחנו בעבר כי $P\subseteq NP$ בנוסף, אם $S\in P$ זה גורר ש $\overline{S}\in P$ כי נוכל בקלות להחזיר תשובה הפוכה ולכן $\overline{S}\in NP$ . סך הכל נקבל $\overline{S}\in CO-NP$

![Pasted image 20230602210735.png|400](/img/user/Assets/Pasted%20image%2020230602210735.png)

## CO-P 
נגדיר את $CO-P$ להיות :

$$COP=\{S \ | \overline{S}\in P\}$$

==משפט-== $COP=P$ .  נוכיח על ידי הכלה דו כיוונית. 
תהי $S\in CO-P$ אזי $\overline{S}\in P$. סך הכל נוכל להכריע את $S$ על ידי החזרת תשובה הפוכה מזו שיחזיר האלגוריתם שמכריע את $\overline{S}$.

__מסקנה__ : אם $NP\neq CONP$ אז $P\neq NP$ 
נב״ש $P=NP$ אזי $COP=CONP$ מטענה קודמת נובע ש 

$$NP=P=COP=CONP$$

לכן $NP=CONP$ בסתירה.

>[!info] השערה
>שיערנו ש $NP\neq CO-NP$ ובנוסף ראינו ש $P\subseteq NP\cap CONP$ ולכן נניח ש $P\subset NP\cap CONP$
>כלומר:
>$$S\in P \leftrightarrow \overline{S}\in P\leftrightarrow \overline{S}\in NP\leftrightarrow S\in CONP$$

## NP/CONP

__טענה__
אם קיימת $A\in NP\cap CONP$ שהיא [[Computer Science/Computation and Complexity/Cook Reduction#NP-קשה*\| NP-קשה*]] אז $NP=CONP$ 

נוכיח בהכלה דו כיוונית  תחת ההנחה הנ״ל

$\subseteq$ :
ראשית , תהי $S\in CONP$ , אזי $\overline{S}\in NP$
לכן, קיימת רדוקציית קוק מ $\overline{S}$ ל $A$ לפי הגדרת בעיה $NP$-קשה* . כמו כן תמיד קיימת רידוקצית קוק מ $S$ ל $\overline{S}$ . 
סה״כ מטרנזיטיביות הרדוקציות נקבל שקיימת רדוקציית קוק מ $S$ ל $A$ .

כעת נוכל לבנות ל $S$ מוודא פולינומי $V_{S}$ (כדי להוכיח שהוא אכן ב $NP$) שאינטואיטיבית יהיה דומה לפעולת רדוקציית הקוק מבלי להחשיב את הקריאות למכונת אורקל שפותרת את $A$ , כי אין לו גישה אליה, במקום זאת, נסמלץ אותן באופן מלאכותי ובכל מקום שקראנו לאורקל והוא החזיר תשובה חיובית, כיוון ש $A\in NP$ , במקום להחזיר רק תשובה חיובית, נחזיר גם עד מתאים  ובאופן דומה אם החזיר תשובה שלילית נחזיר גם עד מתאים ומובטח שהוא קיים כי $A\in CONP$ .

אם כן, המוודא $V_{S}(x,y)$ יקבל עדות $y$ המורכבת מאוסף שלשות מהצורה 

$$y=\{(q_{1},a_{1},w_{1}),\dots, (q_{t},a_{t},w_{t})\}$$

כאשר  $q_{i}$ היא שאלה מספר $i$ שמכונת הרדוקצייה מ $S$ ל $A$ הפעילה לאורקל.
$a_{i}\in \{0,1\}$ היא תשובה לשאלה $q_{i}$ 
$w_{i}$ היא עדות פולינומית לכל ש $a_{i}$ היא אכן תשובת המכונה ל $q_{i}$ .
כלומר אם $a_{i}=1$ אז $w_{i}$ תהיה עדות לכל ש $q_{i}\in A$ ואם $a_{i}=0$ אז $w_{i}$ תהיה עדות לכך ש $q_{i}\notin A$ 

__תיאור המוודא:__
$V_{s}(x,y)$ יריץ את מכונת האורקל מ $S$ ל $A$ ($M^{A}$) , אך יחליף כל פנייה ל $A$ בשימוש בתשובה $a_{i}$ שהתקבלה בעדות, וכן בעדות $w_{i}$ שהתקבלה שמוכיחה שהתשובה שהאורקל החזיר אכן נכונה. כלומר, לפני שימוש בתשובה $a_{i}$ להמשך הביצוע, המוודא יוודא כי התשובה אכן נכונה ועל כן יריץ בצורה נכונה את לוגיקת הרידוקציה אך ללא פנייה ל אורקל של $A$ .
אם כן, מתקיים ש אם $x\in S$ אז יש עדות $y$ שניתן לתת ל $V_{S}$ שמתאים לריצת הרדוקצייה. ואם $x\notin S$  המוודא יחזיר $0$ לכל ריצה על הרדוקצייה שבנינו.

$\supseteq$ :
נב״ש כי $NP\nsubseteq CONP$ , אזי קיימת $S^{\prime}\in NP/CONP$ , ולכן $S^{\prime}\in NP$ וגם $S^{\prime}\notin CONP$ . כלומר $\overline{S^{\prime}}\in CONP$ ומכיוון ש $CONP\subseteq NP$ אזי $\overline{S^{\prime}}\in NP$ ולכן $S^{\prime}\in CONP$ בסתירה לכך ש $NP\nsubseteq CONP$ .

__טענה__ 
אם $CONP$ מכילה בעיה $NPH$ אז $NP=CONP$
כדי להוכיח את הטענה, ראשית נשים לב כי אם $CONP\subseteq NP$ אז גם $NP\subseteq CONP$ כיוון ש 

$$S\in NP\to \overline{S}\in CONP\to \overline{S}\in NP\to S\in CONP$$

ולכן מספיק להראות שאם $CONP$ מכילה בעיה $NPH$ כלומר $S\in CONP\cap NPH$ אזי $CONP\subseteq NP$

![Pasted image 20230701003002.png](/img/user/Assets/Pasted%20image%2020230701003002.png)

==טענת עזר== אם יש רדוקציית קארפ מ $S^{\prime}$ ל $S$ אז יש רדוקציית קארפ מ $\overline{S^{\prime}}$
ל $\overline{S}$.

![Pasted image 20230701121436.png](/img/user/Assets/Pasted%20image%2020230701121436.png)

בגלל שיש רדוקציית קארפ מ $S^{\prime}$ ל $S$ אז ישנה $f$ חשיבה פולינומית המקיימת 

$$\displaylines{
x\in S^{\prime}\leftrightarrow f(x)\in S \\
\downarrow \\
x\notin{S^{\prime}}\leftrightarrow f(x)\notin S \\ \downarrow \\
x\in\overline{ S^{\prime}}\leftrightarrow f(x)\in \overline{S}
}$$

ולכן אותה $f$ תהווה רדוקציית קארפ גם עבור הבעיות המשלימות.

כעת, נחזור ל $S\in CONP\cap NPH$ וניקח בעיה $S^{\prime}\in CONP$ . מתקיים ש 
$\overline{S^{\prime}}\in NP$ . ישנן מספר רדוקציות קארפ שאנחנו יודעים שישנן:
1) מ $\overline{S^{\prime}}$ ל $S$ כי $S$ היא $NPH$ 
2) מטענת העזר יש מ $S^{\prime}$ ל $\overline{S}$ רדוקציית קארפ.

$S\in CONP$ ולכן $\overline{S}\in NP$ . מטענת [[Computer Science/Computation and Complexity/Karp Reduction#סגירות לרדוקציית קארפ\|סגירות לרדוקציית קארפ]] מתקיים ש $S^{\prime}\in NP$ ולכן הראנו שאם הנ״ל מתקיים כל בעיה שנמצאת ב $CONP$ נמצאת גם ב$NP$ ולכן 

$$CONP\subseteq NP$$

כלומר: $NP=CONP$

__משפט :__
בהנחה ש $P\neq NP\cap CONP$ אז קיימת בעיית חיפוש $R\in PC$ שאין לה [[Computer Science/Computation and Complexity/Cook Reduction#רידוקצייה עצמית\|רדוקצייה עצמית]].

![Screenshot 2023-07-01 at 17.12.20.png](/img/user/Assets/Screenshot%202023-07-01%20at%2017.12.20.png)

__הוכחה:__ 
תהי $S\in NP\cap CONP/P$ כלומר $S,\overline{S}\in NP$ .
נסמן ב $V_{S},V_{\overline{S}}, P_{S},P_{\overline{S}}$ את הפולינומים והמוודאים של $S,\overline{S}$. 
נגדיר את בעיות החיפוש הבאות: 

$$\displaylines{
R_{S}= \{(x,y) \ | \ |y|\leq P_{S}(|x|), V_{S}(x,y) = 1\} \\
R_{\overline{S}}= \{(x,y) \ | \  |y|\leq P_{\overline{S}}(|x|) , V_\overline{S} (x,y) = 1\}
}$$

מאיך שהגדרנו את בעיות החיפוש שלנו מתקיים 

$$x\in S \leftrightarrow \exists_{y \, |y|\leq P_{S}(|x|)}: (x,y)\in R_{S} \ \ ,  \ \ \forall_{y}: (x,y)\notin R_{\overline{S}}$$

$$x\in \overline{S} \leftrightarrow \exists_{y \, |y|\leq P_{\overline{S}}(|x|)}: (x,y)\in R_{\overline{S}} \ \ ,  \ \ \forall_{y}: (x,y)\notin R_{S}$$

נגדיר כעת $R= R_{S}\cup R_{\overline{S}}$ ונראה כי $R$ היא בעיית חיפוש ב $PC$ שאין לה רדוקצייה עצמית.

$R\in PC$ : בהנתן $(x,y)$ נריץ את שתי המוודאים ואם לפחות אחד מהם החזיר $1$ נחזיר $1$ אחרת נחזיר $0$.

נרצה כעת להראות של $R$ אין רדוקצייה עצמית, נראה כי $R\notin PF$ ו $S_{R}\in P$ ומטענה קודמת נקבל את הדרוש.

נשים לב ש 

$$S_{R}= S\cup \overline{S}= \{0,1\}^{*}\in P$$

נראה אם כן ש $R\notin PF$ על ידי הנחה בשלילה שהיא כן ב $PF$ .
אם $R\in PF$ אז ישנו אלגוריתם פולינומי $A$ שפותר אותה כלומר בהינתן $x$ יחזור $y$ אם $(x,y)\in R$ . בעצם מתקיים ש $x\in S\leftrightarrow (x,A(x))\in R$ . נבנה אלגוריתם $D$ המכריע את $S$

בהנתן קלט $x$ נריץ את $A(x)$ ונבדוק האם $(x,A(x))\in R_{S}$ , נוכל לעשות זאת בזמן פולינומי :
* אם $A(x)$ החזיר $\perp$ אז אנחנו יודעים שהוא לא ב $R_{S}$ כי הוא לא ב$R$.
* אם $A(x)$ החזיר $y$ כלשהו פשוט נריץ את המוודא $V_{S}(x,y)$ ונבדוק את התוצאה. 

$D$ אם כן, אלגוריתם פולינומי המקיים 

$$x\in S \leftrightarrow (x,A(x))\in R_{S}\leftrightarrow D(x)=1$$

סה״כ הראנו כי $R\in PC$ ואין לו רדוקצייה עצמית.


## הגדרה שקולה ל $CONP$
$S\in CONP$ אם קיים פולינום $P$ ומוודא פולינומי $V$ כך ש 

$$x\in S \leftrightarrow \forall_{y : |y|\leq P(|x|)}: V(x,y)=1$$

למשל עבור $\overline{SAT}$ המוודא יקבל נוסחה והשמה ויחזיר $1$ אמ״מ ההשמה לא מספקת.