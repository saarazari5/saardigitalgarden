---
{"dateCreated":"2023-02-02 00:23","tags":["abstract_algebra","computer_science"],"pageDirection":"rtl","dg-publish":true,"permalink":"/computer-science/algebraic-structure/permutations/","dgPassFrontmatter":true}
---


תמורה היא פונקצייה חח״ע ועל $\sigma: [n]\rightarrow[n]$ . אוסף התמורות על $n$ איברים מסומן כ $S_{n}$ ו[[Computer Science/Discrete Math/Cardinal number\|עוצמתו]] היא $n!$ .
$S_{n}$ נקרא גם [[Computer Science/Algebraic Structure/Permutations groups\|חבורת התמורות]].

דוגמה לתמורה מעל $S_{3}$ למשל תהיה :
$$\displaylines{
\sigma:\{1,2,3\}\to \{1,2,3\}\\
\sigma(1)=2\\\sigma(2)=1 \\\sigma(3)=3
}$$

>[!info] תמורת הזהות
>נסמן אותה כ$I$ שזה גם הסימון של פונקציית הזהות וזאת פשוט תמורה שבה כל איבר הולך לעצמו


## דרכים מקובלות לסמן תמורה
__כפונקצייה:__
$$\begin{pmatrix}1&2&3&\dots&n\\ \sigma(1)&\sigma(2)&\sigma(3)&\dots&\sigma(n)\end{pmatrix}$$

__כמחזורים זרים__ :
אדגים זאת כאן אבל פירוט נוסף נמצא בהסבר על [[Computer Science/Algebraic Structure/Permutations groups\|חבורת התמורות]]
למשל עבור התמורה
$$\begin{pmatrix}1&2&3\\2&1&3\end{pmatrix}$$
נוכל לסמן אותה גם כ 
$$\sigma=(1,2) \ (3)$$
זה בעצם סימון שמפריד את התמורה לחלקים שקוראים אחד לשני... 

## חילופי סדר

בתמורה ישנו מצב של חילוף סדר שמשמעו 
 $$(i<j)\wedge(\sigma(i)>\sigma(j))$$

### דרכים למציאת חילופי הסדר
__מעקב אחרי הערכים (זאת שיטה פחות יעילה):__
למשל עבור $\sigma(1,3,2)$  נבנה טבלה של כל הערכים שקטנים אחד מהשני ומה קורה כשמפעלים את הפרמוטצייה:

| 1<2                   | 1<3                   | 2<3                   |
| --------------------- | --------------------- | --------------------- |
| $\sigma(1)>\sigma(2)$ | $\sigma(1)>\sigma(3)$ | $\sigma(2)<\sigma(3)$ |

ולכן מספר חילופי הסדר בתמורה הוא $2$

__השיטה הציורית__
1) נכתוב את התמורה כפונקצייה ונעביר ״קו״ בין מספרים שווים
2) נספור את החיתוכים שנוצרו לנו ואלו מספר חילופי הסדר
![Pasted image 20230202005014.png|200](/img/user/Assets/Pasted%20image%2020230202005014.png)

## סימן התמורה

לכל תמורה יש סימן התלוי במספר חילופי הסדר, נסמן את מספר חילופי הסדר ב $k$ ויתקיים
 $$sign(\sigma)=(-1)^{k}$$

>[!info] זוגיות ואי זוגיות של תמורה:
>תמורה תקרא זוגית אם $sign(\sigma)=1$  ואי זוגית אם $sign(\sigma)=-1$ 

__הגדרה נוספת לסימן התמורה__ : 
נגדיר את סימן התמורה $f$ להיות

$$\prod_{i\neq j} \frac{f(i)-f(j)}{i-j}$$

למשל עבור התמורה 

$$f=\begin{pmatrix}1 & 2 & 3 \\ 3 & 1 & 2\end{pmatrix}$$

יתקיים 

$$sign(f)= \frac{3-1}{1-2}\cdot \frac{3-2}{1-3}\cdot \frac{1-2}{2-3}= (-1)(-1)=1$$

