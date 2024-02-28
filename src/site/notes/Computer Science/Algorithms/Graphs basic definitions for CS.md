---
{"tags":["algorithms","computer_science"],"dg-publish":true,"pageDirection":"rtl","permalink":"/computer-science/algorithms/graphs-basic-definitions-for-cs/","dgPassFrontmatter":true}
---


# Graphs basic definitions for CS

## גרף
גרף הוא זוג סדור $G=(V,E)$ כאשר $V$ היא קבוצת קודקודים ו $E$ קבוצת הקשתות - $E=\{\{u,v\}\ | \ u,v\in V\}$
לגרף פשוט (גרפים שאין בהם לולאות כלומר קשת מקודקוד לאותו קודקוד, ואין קשתות מקבילות כלומר הקשת מ א ל ב שקולה לקשת מ ב ל א. ) עם $n$ קודקודים יש לכל היותר $\binom{n}{2}$ קשתות. 

## גרף מכוון
גרף הוא __מכוון__ אם סדר הקודקודים בקשת משנה , כלומר עבור קשת $e\in E$ שמקשת $u,v$ היא זוג סדור $(u,v)$ . נאמר ש $e$ מכוונת מ $u$ ל $v$
![Pasted image 20221128115421.png|100](/img/user/Assets/Pasted%20image%2020221128115421.png)
בגרף מכוון ייתכן מקרה שבו קשת יוצאת מקודקוד לזה, זה נקרא __self loop__ 
## גרף לא מכוון
גרף הוא __לא מכוון__ אם סדר הקודקודים בקשת לא משנה , עבור קשת $e=\{u,v\}$ 
![Pasted image 20221128132722.png|100](/img/user/Assets/Pasted%20image%2020221128132722.png)
בגרף לא מכוון אין לולאות עצמיות.

## הדרגה של קודקוד
### בגרף לא מכוון
הדרגה של $u\in V$ בגרף לא מכוון היא מספר הקשתות שמכילות את $u$ . סימון- $deg(u)$ ומתקיים  (למת לחיצת הידיים)
$$\sum\limits_{u\in V}deg(u)= 2|E|$$
### בגרף מכוון
__הדרגה היוצאת__ של קודקוד $u\in V$ הוא מספר הקשתות __מ__ $u$ ומסמנים $outdeg(u)$ . באופן דומה, __הדרגה הנכנסת__ של קודקוד $u\in V$ היא מספר הקשתות __ל__ $u$ ומסומנת $indeg(u)$ ומתקיים
$$\sum\limits_{u\in V} indeg(u) = \sum\limits_{u\in V} outdeg(u)= |E|$$

## קודקודים שכנים 
בגרף מכוון ולא מכוון , אם יש קשת בין $u,v$ נאמר שהם __שכנים__ .

## מסלול
מסלול $P$ בגרף $G(V,E)$ (מכוון או לא מכוון) הוא סדרה של קדוקדוים $P=(v_{0},v_{1},\dots, v_{n})$ כאשר $\forall_{0\leq i\leq n-1}:(v_{i},v_{i+1})\in E$
ניתן לייצר מסלול גם על ידי קשתות $P=(e_{1},e_{2},\dots, e_{n})$ כאשר $\forall_{1\leq i\leq k} : (v_{i-1},v_{i})= e_{i}$. במקרה זה נאמר שהמסלול הוא מאורך $n$ (מספר הקשתות)

1) אם לכל $i\neq j$  מתקיים $v_{i}\neq v_{j}$ נאמר ש __המסלול פשוט__ .
2) אם $v_{0}=v_{n}$ נאמר ש $P$ מהווה __מעגל__ 
3) _תת מסלול_ $P^{\prime}$ של $P$  $P^{\prime}=(v_{i},v_{i+1},\dots, v_{j})$ כאשר $0\leq i\leq j\leq n$ .
4) _המסלול ההפוך_ של $P$ מסומן $P^{R}=(v_{n},v_{n-1},\dots, v_0)$

## רכיב קשירות
__בגרף לא מכוון__ $G=(V,E)$ רכיב קשירות של $G$ הוא תת [[Computer Science/Algorithms/Graphs basic definitions for CS#גרף קשיר\|#גרף קשיר]] מקסימלי, כלומר קבוצת קודדקודים מקסימלית $C\subseteq V$ כך שלכל זוג קודקודים $u,v\in C$ קיים מסלול מ $u$ ל $v$ ב $G$.

![Pasted image 20221128144653.png|250](/img/user/Assets/Pasted%20image%2020221128144653.png)

## גרף קשיר
גרף לא מכוון 𝑮 נקרא קשיר אמ"מ 𝑉 רכיב קשירות. 
![Pasted image 20221128144759.png|250](/img/user/Assets/Pasted%20image%2020221128144759.png)

## רכיב קשירות חזק
עבור גרף מכוון $G=(V,E)$ , רכיב קשירות חזק ב-𝐺 הוא קבוצה מקסימלית של קודקודים $C\subseteq V$ כך שלכל זוג קודקודים $u,v\in C$, קיים מסלול  מ $u$ ל $v$  ומסלול מ $v$ ל $u$ ב $G$

![Pasted image 20221128145318.png|450](/img/user/Assets/Pasted%20image%2020221128145318.png)

## גרף קשיר חזק
גרף מכוון $G=(V,E)$ נקרא קשיר חזק אמ"מ 𝑉 רכיב קשירות חזק .

![Pasted image 20221128145604.png|250](/img/user/Assets/Pasted%20image%2020221128145604.png)

## יער ועצים 
__יער__ : גרף לא מכוון __ללא מעגלים__

__עץ__ : _יער_ קשיר

__עלה__ : ביער עלה הוא קודקוד עם דרגה 1.

![Pasted image 20221128145724.png|250](/img/user/Assets/Pasted%20image%2020221128145724.png)

## תכונות
1) אם גרף $G=(V,E)$ הוא קשיר, אז $|E|\geq |V|-1$
נוכיח באינדוקצייה על מספר הקודקודים:
__בסיס__ :
עבור $|V|=0$ אין קשתות ולכן $|E|= 0 \geq 0-1=-1$ .
עבור $|V|=1$ , גרף קשיר שזה רק קודקוד אחד ולכן מתקיים באותו אופן כמו למעלה.
__צעד__:
נניח שהטענה נכונה עבור מספר קודקודים לפחות 2 כלומר $|V|=n\geq 2, |E|\geq n-1$ ונוכיח עבור גרף קשיר עם $n+1$ קודקודים כלומר $|E|\geq n$
נב״ש שמספר הקשתות $E$ קטן מ $n$. 
לפי למת לחיצת הידיים מתקיים 

$$\sum\limits_{v\in V} deg(v) < 2n$$

ז״א שקיים לפחות קודקוד אחד $u$ כך ש $deg(u)\leq 1$ (כי יש $n+1$ קודקודים)
* אם $deg(u)=0$ זה סתירה לכך ש $G$ קשיר
* אם $deg(u)=1$ אז נחלק את הגרף לשתי רכיבי קשירות אחד עם $n$ קודקודים והשני עם קודקוד בודד. בגלל שהורדנו קשת אחד כדי ליצור שתי רכיבי קשירות אז הרכיב קשירות עם $n$ קודקודים מכיל $|E|-1$ קשתות. כמו כן זהו גרף קשיר אחרת $G$ עצמו לא היה קשיר וזה יהיה בסתירה לנתון. כיוון שהוא קשיר ומכיל $n$ קודקודים, מהנחת האינדוקצייה יתקיים 
 
 $$|E|-1 \geq n-1 \rightarrow |E|\geq n$$
 
לא הגיוני שיש יותר או מספר זהה של קשתות מקודקודים בגרף לא מכוון __בסתירה__ .

2) יהי $G(V,E)$ גרף קשיר לא ריק אזי: $G$ עץ אמ״מ $|E|=|V|-1$


3)  יהי $G(V,E)$ גרף עם $k$ רכיבי קשירות אזי:  $G$ יער אמ״מ $|E|=|V|-k$

4) יהי $G=(V,E)$ גרף לא מכוון אזי $G$ עץ אמ״מ עבור כל זוג קודקודים קיים בידיוק מסלול פשוט אחד בינהם.

5) עבור גרף $T=(V,E)$ עץ סופי עם $|V|\geq 2$ , עץ זה חייב להכיל עלה.
נניח בשלילה שלא קיים עלה ב$T$ ויהיו $P=(v_{0},v_{1},\dots,v_{k})$  עם $k$ קשתות המסלול הארוך ביותר ב$T$ . נתבונן ב $v_{0}$ , לפי ההנחה הוא לא עלה ולכן דרגתו גדולה או שווה ל2, כלומר ישלו לפחות 2 שכנים שאחד מהם הוא $v_{1}$ . נסמן את השני ב $w$ .
נשים לב ש $w\notin P$  אחרת היה מעגל בעץ וזו סתירה. כלומר נוכל להאריך את $P$ על ידי הגדרתו באופן הבא

$$P=(w,v_{0},v_{1},\dots,v_{k})$$

וקיבלנו מסלול ארוך יותר בגודל $k+1$ בסתירה לכך ש $k$ הוא אורך המסלול הארוך ביותר.

## עץ פורש
עץ פורש לגרף קשיר $G=(V,E)$ הוא עץ $T=(V,E_{T})$ כאשר $E_{T}\subseteq E$ כלומר $T$ הוא תת גרף של $G$ שהוא עץ.

## מולטי גרף
אם קבוצת הקשתות היא [multi-set](https://en.wikipedia.org/wiki/Multiset) , כלומר קשתות יכולות להופיע יותר מפעם אחת, אז הגרף $G$ נקרא מולטי גרף.
במולטי גרף ייתכנו יותר מקשת אחת בין שני קודקודים, גם אם הקשתות לא מכוונות.
![Pasted image 20221128164414.png|150](/img/user/Assets/Pasted%20image%2020221128164414.png)

## ייצוג גרפים בקוד
### מטריצת שכנויות adjacency matrix 
מייצגים את הגרף על ידי מטריצה בוליאנית בגודל $|V|\times |V|$ כאשר התא $(i,j)$ הוא $1$ אמ״מ קיימת קשת בין $v_{i}$ ל $v_{j}$ ב $G$.
נשים לב שאם הגרף לא מכוון נקבל מטריצה סימטרית, בנוסף האלכסון כולו אפסים כי לא תתכן קשת מקודקוד לעצמו.
הייצוג הזה משתמש ב $O(|V|^{2})$ זכרון.

![Pasted image 20221128164728.png](/img/user/Assets/Pasted%20image%2020221128164728.png)

### רשימת שכנויות adjacency list 
מייצגים את הגרף ע"י מערך של רשימות , המערך בגודל $|V|$ והתא ה $i$ במערך מצביע לרשימה של השכנים של $v_{i}$ , משתמשים בזכרון $O(|V|+|E|)$

![Pasted image 20221128165704.png](/img/user/Assets/Pasted%20image%2020221128165704.png)

### זמני ריצה 

|                          | מטריצת שכנויות | רשימת שכנויות                  |
| ------------------------ | -------------- | ------------------------------ |
| מקום                     | $O(V^{2})$     | $O(V+E)$                       |
| האם $(u,v)\in E$         | $O(1)$         | $O(\log(\min(deg(u),deg(v))))$ |
| מעבר על כל שכני $u\in V$ | $O(V)$         | $O(deg(u))$                    |
| סריקת כל הקשתות          |       $O(V^{2})$         |  $O(V+E)$                              |



