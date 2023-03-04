---
{"dateCreated":"2023-03-01 17:48","tags":["abstract_algebra","computer_science"],"pageDirection":"rtl","dg-publish":true,"permalink":"/cs/algebraic-structure/encoding/","dgPassFrontmatter":true}
---



# קידוד
מטרת הקידוד היא זיהוי ותיקון שגיאות בתהליך העברת המידע .דוגמא: כאשר נשלח מידע כלשהו (רצף של ביטים), נרצה לדעת שלא נפלה טעות בדרך. לשם כך, נוכל לשלוח את אותו המידע פעמיים. כך, מובטח שאם ביט אחד שובש, שתי פיסות המידע לא יהיו תקינות ונוכל לזהות שגיאה. אך, לא נוכל לדעת לתקן את השגיאה. לשם כך, נוכל לשלוח שלושה רצפים אחד אחרי השני, ובכך אם נפלה טעות אחד נוכל לתקן. נשים לב שזה לא פרקטי על מנת לזהות ולתקן שגיאות היינו צריכים להכפיל ואף לשלש את המידע.


## קוד, מילה ויתירות
1) __קוד__ הוא תת קבוצה של $\mathbb{Z}_{2}^{m}$ כלומר קבוצה של וקטורים בינאריים באורך $m$. 
2) __מילה__ היא איבר בקבוצה $x\in\mathbb{Z}_{2}^{m}$ . מילה תמיד מורכבת מ __יתירות__+__מידע__ כאשר המידע והיתירות הן וקטורים בינארים מגדלים $k,n$ כך ש $k+n=m$ . 

__מילה נקראת חוקית__ כאשר היא קידוד של מידע כלשהו, כלומר ישנו איזשהו קופסה שחורה ״המקודד״ שיודע לקחת את המילה ולחלץ ממנה מידע. מילה לא חוקית משמעותה שנפלה שגיאה.

## parity check 
מזהה האם כמות השגיאות היא אי זוגית. אם היא זוגית לא נזהה את השגיאה. 
נניח שהמידע שלנו הוא $v=(x_{1},x_{2}\dots,x_{n})$ ונגדיר את המילה המקודדת כ $u=\left(x_{1}\dots,x_{n}, \sum\limits_{i=1}^{n}x_{i}\right)$ כאשר הסכום הוא היתירות. לאחר העברת מידע ישנה מילה שמתקבלת בצד השני, נסמנה כ $u^{\prime}=(y_{1},y_{2},\dots,y_{n+1})$ . נשים לב ש: 

$$x_{1}+\dots+x_{n}+\sum\limits_{i=1}^{n}x_{i}=2\sum\limits_{i=1}^{n} x_{i}\equiv_{2}0$$

==חשוב: כל המתמטיקה היא מעל חוג השאריות מסדר 2==
אם המילה המתקבלת שווה למילה שנשלחה כלומר $u=u^{\prime}$ יתקיים

$$\sum\limits_{i=1}^{n}y_{i}=2\sum\limits_{i=1}^{n}x_{i}\equiv_{2}0$$

### שגיאה
נזכר ש $e_{i}=(0,\dots,\underbrace{1}_{i},\dots 0)$ ונגדיר __שגיאה__ באינדקס $i$ להיות $u+e_{i}$ . כעת במצב של שגיאה אחת יתקיים $u^{\prime}=u+e_{i}$ כלומר 

$$u^{\prime}=x_{1}+\dots + (x_{i}+1)+\dots + \sum\limits_{j=1}^{n}x_{j}= 1$$

כלומר ניתן לראות שבמצב שבו יש שגיאה אחת (או מספר שגיאות __אי זוגי__), התוצאה של הסכימה תהיה $1$ ולכן נוכל לזהות שגיאות כאשר מספר השגיאות הוא אי זוגי. אם לעומת זאת מספר השגיאות הוא זוגי נקבל בסכימה $0$ שזה כמו שנקבל במצב שאין שגיאות בכלל ולכן לא נזהה.

## קידוד ליניארי
נגדיר קידוד ליניארי עם מטריצות. נסתכל על $A\in\mathbb{Z}_{2}^{k\times m}$ . ניצור ממנה את זוג המטריצות הבא

$$G =\binom{I_{m}}{A}  \ \ \ \ H=(A \ \ I_{k})$$

כאשר $G$ היא המטריצה המקודדת ו $H$ היא המטריצה המפענת.

המידע הוא $v\in\mathbb{Z}_{2}^{m}$ כלומר וקטור בגודל כמות עמודות המטריצה $A$ והיתירות היא באורך $k$ כמות שורות המטריצה $A$ .
המילה המקודדת היא $G\cdot{v}$ כלומר 

$$u=G\cdot v = \binom{I}{A}v= \binom{v}{Av}$$

כעת בנינו את המילה הנשלחת. איך מקודדים אותה ומזהים שגיאות?

>[!info] הבחנה
>אוסף המילים החוקיות הוא $Im(G)$ כלומר כל המילים $Gv$ 

הצד השני בדומה לשיטה של בדיקת זוגיות, מקבל $u^{\prime}$ ומחשב $H\cdot u^{\prime}$ במטרה לקבל וקטור $0$.
__טענה:__ תהי מילה $u\in\mathbb{Z}_{2}^{k+m}$  אזי

$$\exists_{v\in\mathbb{Z}_{2}^{m}}: u=Gv\leftrightarrow Hu=0$$
כלומר- __קוד ליניארי__ הוא בעצם $N(H)$ כלומר מרחב האפס של המטריצה $H$.

_הוכחה-_ 
$\leftarrow$ : נניח $u=Gv$ אזי 

$$HG=(A \ I)\binom{I}{A}=AI+AI=2AI\equiv_{2}0$$

סך הכל נקבל שלכל $v$

$$HGv=0v=0$$

$\rightarrow$ : נניח $Hu=0$ , יהי $v$ כלשהו והמילה הנשלחת $u=\binom{v}{x}$ כדי ש $u$ תהיה מילה חוקית צריך להראות ש $x= Av$ . אכן:

$$Hu=(A\ \ I)\binom{v}{x}= Av+x=0\overset{\text{in }\mathbb{Z}_{2}}{\to} Av=x$$

>[!info] נשיב לב
>מסקנה מההוכחה הזאת היא שיתירות של מילה $u$ היא יחידה והיא תמיד $Av$ 

__דוגמה:__
$A=\begin{pmatrix}1 & 1 & 1 & 1\end{pmatrix}$ , נשים לב שכמות העמודות = אורך המידע , כמות השורות = אורך היתירות, אם כן 

$$G=\begin{pmatrix}1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1 \\ 1 & 1 & 1 & 1 \end{pmatrix}$$

ו $v=\begin{pmatrix}a \\ b \\ c \\ d\end{pmatrix}$ ומתקיים

$$G\cdot v= G\begin{pmatrix}a \\ b \\ c \\ d\end{pmatrix}=\begin{pmatrix}a \\ b \\ c \\ d \\ a+b+c+d\end{pmatrix}$$

__כיצד שגיאות משפיעות על התהליך?__
נניח והתקבלה שגיאה אחת כלומר המילה שהגיע לצד השני היא $u^{\prime}=u+e_{i}$ ויתקיים 

$$Hu^{\prime}= H(u+e_{i})=Hu+He_{i}= C_{i}(H)$$

כלומר נזהה כל שגיאה יחידה אמ״מ ב $H$ __אין__ עמודות אפסים (שזה שקול לדרישה שב $A$ לא יהיו עמודות $0$). אם למשל העמודה ה $i$ ב $H$ הייתה $0$ במקרה הנ״ל עדיין היינו מקבלים בתוצאה $0$ שזה לא טוב.

__מה לגבי שתי שגיאות?__ $e_{i},e_{j}$ יתקיים

$$Hu^{\prime}= H(u+e_{i}+e_{j})= C_{i}(H)+C_{j}(H)$$

אם אלו היו זהות אז היינו מקבלים $0$ גם כן בגלל שאנחנו מעל השדה מודולו 2. ולכן נזהה 2 שגיאות אמ״מ ב $H$ אין זוג עמודות זהות.

>[!info] מסקנה
>אם אין עמודות אפסים ב $H$ ואין 2 עמודות זהות, במצב זה נזהה כל 2 שגיאות ונוכל __לתקן__ שגיאה יחידה 

__שאלה:__ 
בהינתן $k$ ביטי יתירות מה יהיה אורך המידע הגדול ביותר עבורו $H$ תהיה ללא עומדת אפסים וללא זוג עמודות זהות? 
זוהי שאלה [[CS/discrete math/combinatorics basics\|קומבינטורית]] , אם יש $k$ ביטי יתירות זה אומר שיש $k$ שורות ב $A$ כלומר כל עמודה יכולה להחזיק בתוכה $k$ ערכים ולכן יש $2^{k}$ צירופים שונים לבניית עמודה (כולל עמודות האפסים.) כמו כן נשים לב ש $H$ מורכבת מ $k$ עמודות יחידה ולכן גם הן תפוסות לנו, סך הכל אורך המידע הגדול ביותר עבורו $H$ תהיה ללא עמודות אפסים וללא זוג עמודות זהו הוא 

$$m=2^{k}-1-k$$

__דוגמה__ נניח שיש 3 ביטי יתירות האפשרויות לעמודות של $A$ הן

$$A=\begin{pmatrix}1 & 0 & 1 & 1 \\ 1 & 1 & 0 & 1   \\ 0 & 1 & 1 & 1 \end{pmatrix}$$

כי $2^{3}-1-3=4$ . ולכן $H$ תיראה ככה

$$H=\begin{pmatrix}1 & 0 & 1 & 1 & 1 & 0 & 0 \\ 1 & 1 & 0 & 1 & 0 & 1 & 0  \\ 0 & 1 & 1 & 1 & 0 & 0 & 1  \end{pmatrix}$$

### מרחק המינג 
[[CS/algorithms/FFT#בעיית חישוב מרחקי האמינג בין טקסט לתבנית\|מרחק המינג]] היא בעייה ידועה בעולם האלגוריתמים. נוכל לראות איך היא עוזרת לנו לפתור בעיות הקשורות לקידוד. 

__משקל המינג:__
נגיד משקל המינג של וקטור $v\in\mathbb{Z}_{2}^{n}$ להיות מספר האחדות שבו. 
__מרחק המינג__:
מרחק המינג $d(u,v)$ בין שני וקטורים הוא מספר השורות השונות בינהם. בגלל שאנחנו מעל שדה המודולו 2, ניתן לחשב את הנ״ל על ידי חישוב משקל המינג של $u-v$ .

_דוגמה-_
עבור $\begin{pmatrix}1 & 1 & 0 & 0\end{pmatrix},\begin{pmatrix}0 & 1 & 1 & 1\end{pmatrix}$ יתקיים שמרחק המינג שלהם הוא 

$$(1\ 1\ 0 \ 0)-(0\ 1\ 1\ 1)=(1\ 0\ 1\ 1)$$

__הגדרה__- המרחק $d_{min}$ של קוד הוא המרחק המינימלי בין שתי מילות קוד שונות. 

__טענה 1-__ בקוד ליניארי המרחק $d_{min}$ שווה למשקל המינימלי של מילות קוד שאינן וקטור האפס.
__טענה 2-__ יהי $C$ קוד ליניארי עם מרחק $d_{min}$. __אם__ $d_{min}\geq 2d+1$ כאשר $d$ מספר כלשהו אז $C$ יצליח לזהות $2d$ שגיאות ולתקן $d$ שגיאות __אמ״מ__ אין ב $H$ המטריצה שהמילות החוקיות שייכות למרחב ה$0$ שלה היא מטריצה בלי עמודת 0 ובלי עמודות זהות.  

__דוגמה-__
תהי המטריצה 

$$H=\begin{pmatrix}1 & 1 & 1 & 0 & 0 \\ 1 & 0 & 0 & 1 & 0 \\ 1 & 1 & 0 & 0 & 1\end{pmatrix}$$

נחשב את $d_{min}$ של הקוד שהוא מרחב האפסים של $H$ . ונראה כמה שגיאות ניתו להזות ולתקן.

נשים לב שסכימה של העמודות $C_{1}+C_{2}+C_{4}$ תניב $0$ כלומר יש וקטור $v$ ששייך למרחב האפסים של $H$ (כלומר הוא מילת קוד) והוא

$$Hv=\begin{pmatrix}1 & 1 & 1 & 0 & 0 \\ 1 & 0 & 0 & 1 & 0 \\ 1 & 1 & 0 & 0 & 1\end{pmatrix}\begin{pmatrix}1 \\ 1 \\ 0 \\ 1 \\ 0\end{pmatrix}=\overline{0}$$

המשקל של וקטור זה הוא $3$ ולכן $d_{min}\leq 3$ .  לפי הטענה הקודמה אנחנו יודעים ש $d_{min}\geq 3$ אמ״מ אין במטריצה עמודת אפסים או עמודות זהות וזה בידיוק המצב אצלנו לכן $d_{min}=3$ . כלומר במצב זה ניתן לזהות עד שתי שגיאות ולתקן שגיאה אחת. 

__כיצד מתקנים שגיאה?__ 
נניח ואירעה שגיאה אחת בידיוק במילת קוד $v$.. כפי שאמרנו התוצאה שנקבל היא העמודה ה $i$ של $H$ ולכן נוכל לדעת שהשגיאה קרתה בסיבית ה $i$ של $v$ וכדי לתקן נוכל פשוט להחזיר לצד המקבל את 

$$(v+e_{i})+e_{i}\equiv_{2}v$$


## קידוד פולינומי
נעסוק כעת ב[[CS/Algebraic structure/groups#חוג\|חוג]] הפולינומים מעל $\mathbb{Z}_{2}$ . איבר בחוג נראה ככה 

$$a_{0}+a_{1}x+\dots+ a_{n-1}x^{n}$$

כאשר המקדמים הם בינאריים.
נתרגם בין וקטורים בינאריים לפולינומים וההיפך לפי המקדמים למשל 

$$x^{4}+x^{3}+x \leftrightarrow 11010$$

כלומר מידע הוא מחרוזת בינארית שמתורגמת לפולינום כזה. אם מעבירים $k$ ביטים של מידע אז דרגת פולינום המידע היא $k-1$ .

___משפט___ : יהי $f,g$ פולינומים מעל $\mathbb{Z}_{2}[x]$ אזי קיימים פולינומים יחידים $q,r$ כך ש 

$$f=qg+r$$
ומתקיים $\deg(r)<\deg(g)$.

### תהליך הקידוד הפולינומי
יהי $g$ פולינום מדרגה $m$ ($m$ מייצג את אורך היתירות) ויהי $f$ פולינום מידע ומדרגה חסומה $k-1$ .
נחשב 

$$x^{m}\cdot f$$

נחלק 

$$x^{m}f \neg g$$

היתירות היא __שארית החלוקה__ $r(x)$ כלומר היתירות יחידה. סך הכל המילה היא 

$$x^{m}f+r$$

__טענה:__ מילים הן חוקיות אמ״מ הן מתחלקות ב $g$ . 
_הוכחה-_ 

$$x^{m}f = qg+r \to x^{m}f+r=qg$$

## קודים ציקליים
יהי וקטור בינארי $(a_{0},a_{1},\dots,a_{n-1})$ הזזה ציקלית שלו היא $(a_{n-1},a_{0},a_{1},\dots,a_{n-1})$ .
קוד נקרא __ציקלי__ אם כל הזזה ציקלית של מילה חוקית היא גם מילה חוקית.

>[!note] למה צריך קוד ציקלי?
>היתירות עבור כל מילה היא ייחודית. ולכן כל שגיאה תהפוך את היתירות להיות יתירות שלא מתאימה למילה. מכיוון שניתן לעשות הזזות בקוד ציקלי נוכל להעביר את 𝑚 הביטים עם הטעות להיות ביטי היתירות וכך נוכל לזהות שישנה שגיאה.

__למשל:__
עבור $01101001$ הזזה ציקלית אחת תביא אותנו ל $11010010$ ומתקיים

$$x^{7}+x^{6}+x^{4}+x= x(x^{6}+x^{5}+x^{3}+1)$$

>[!info] נשים לב
>לא מבצעים את ההזזות בפועל כי כשנגדיל את הפולינומים יהיו המון הזזות ונרצה דרך שונה לבדוק האם מילה היא חוקית או לא. אנחנו רק הוכחנו שאם ההזזה תביא למילה לא חוקית אז ניתן לזהות שגיאות.

__טענה__: עבור קוד ציקלי ומילה חוקית $u$ נניח ש $u^{\prime}$ התקבלה על ידי טעויות בטווח של $m$ ביטים. אזי $u^{\prime}$ איננה חוקית.
_הוכחה-_ נב״ש ששתיהן חוקיות, נבצע הזזות של $u$ ו $u^{\prime}$ כך שכל השגיאות ב $u^{\prime}$ יהיו ב $m$ הביטים האחרונים. נקבל שתי מילים חוקיות הנבדלות רק ביתירות, __בסתירה__ .

__טענה__: הכפלה של פולינום ב $x^{k}$ שקולה לביצוע shift על המחרוזת הבינארית.

### הזזה ציקלית באופן אלגברי
נתאר את פעולת ההזזה הציקלית באופן אלגבי: $u=a_{n-1}x^{n-1}+\dots+a_{0}$ .
1) אם $a_{n-1}=0$ אז ההזזה היא $w=a_{n-2}x^{n-1}+\dots+a_{0}x$ ומתקיים $w=xu$ .
2) אם $a_{n-1}=1$ אז  $xu=x^{n}+a_{n-2}x^{n-1}+\dots+a_{0}x$ , יש $x^{n}$ מיותר וחסר אחד ולכן ההזזה הציקלית הנכונה היא $xu+x^{n}+1=a_{n-2}x^{n-1}+\dots+a_{0}x+1$ .

__משפט:__ יהי $g$ מדרגה $m$ ונביט בקידוד של $k$ ביטי מידע. נסמן $n=k+m$ ויתקיים 
הקוד ציקלי אמ״מ $g$ מחלק של $x^{n}+ 1$ 