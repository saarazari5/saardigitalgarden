---
{"dateCreated":"2023-01-26 10:50","tags":["probability","computer_science"],"pageDirection":"rtl","dg-publish":true,"permalink":"/computer-science/probability/independence-on-continuous-random-variables/","dgPassFrontmatter":true}
---



# אי תלות בין משתנים מקריים רציפים

בדומה ל [[Computer Science/Probability/Independence on discrete random variables\|מקרה הבדיד]] נאמר ששתי [[Computer Science/Probability/Continuous Random Variables\|משתנים מקריים רציפים]]  $X,Y$  הם __בלתי תלויים__ אם ה [[Computer Science/Probability/Joint PDF and CDF\|PDF המשותף]] שלהם הוא תוצר של הPDF השוליים: 

$$f_{X,Y}(x,y)= f_{X}(x)f_{Y}(y)$$
מהנוסחה של [[Computer Science/Probability/Conditioning - continuous random variables#PDF מותנה ב PDF אחר\| PDF מותנה]]  $f_{X,Y}(x,y)=f_{X|Y}(x|y)f_{Y}(y)$ אנחנו למדים שמשמעות הדבר באי תלות היא ש

$$f_{X|Y}(x|y)= f_{X}(x)$$

ובאופן סימטרי

$$f_{Y|X}(y|x)= f_{Y}(y)$$
>[!info]
>מה שזה אומר בעצם היא שלכל ערך $y$ או $x$ שניקח בהתאמה לפונקציות הנ״ל, __החתיכה__ שנקבל שקולה ל $f_{X}(x)$ או $f_{Y}(y)$ בהתאמה.

ההכללה למקרה של יותר משתי משתנים היא די אינטואיטיבית למשל עבור המקרה של 3 משתנים נאמר :

$$f_{X,Y,Z}(x,y,z)= f_{X}(x)f_{Y}(y)f_{Z}(z)$$

אם $X,Y$ הם בלתי תלויים, אז כל שתי מאורעות מהצורה $\{X\in A\}\text{ and }\{Y\in B\}$ הם בלתי תלויים. אכן,

$$\displaylines{ P(X\in A , Y\in B) =  \int_{X\in A}\int_{Y\in B} f_{X,Y}(x,y)dxdy \\ 
= \int_{X\in A}\int_{Y\in B} f_{X}(x)f_{Y}(y)dxdy \\
= \int_{X\in A}f_{X}(x)dx \int_{Y\in B} f_{Y}(y)dy \\ 
= P(X\in A)P(Y\in B)

}$$

באופן ספציפי נקבל מהנ״ל 

$$F_{X,Y}(x,y)= P(X\leq x, Y\leq y)= P(X\leq x) P(Y\leq y)= F_{X}(x)F_{Y}(y)$$

בגלל הקשר של [[Computer Science/Probability/CDF\|CDF]] למשתנה מקרי רציף ובדדי כאחד, אנחנו יכולים להגיד שהנ״ל מהווה איזשהו משפט הגדרה לאי תלות בין שתי משתנים באופן כללי , בלי תלות לסיווגם. 

תכונה חשובה נוספת היא ש 

$$E[g(X)h(Y)]= E[g(X)]E[h(Y)]$$
ומזה כמובן משתמע המקרה הפרטי של פונקצייה הזהות

$$E[XY]=E[X]E[Y]$$

## משתנים נורמלים בלתי תלויים
יהיו $X,Y$ [[Computer Science/Probability/Normal Random Variable\|משתנים נורמלים]] בלתי תלויים עם תוחלות $\mu_{x},\mu_{y}$ בהתאמה ושונות $\sigma^{2}_{x}, \sigma^{2}_{y}$ בהתאמה. נקבל הפונקציית הצפיפות המשותפת תהיה 

$$f_{X,Y}(x,y)= f_{X}(x)f_{Y}(y)= \frac{1}{2\pi \sigma_{x}\sigma_{y}}\cdot e^{- \frac{(x-\mu_{x})^{2}}{2\sigma^{2}_{x}}- \frac{(y-\mu_{y})^{2}}{2\sigma^{2}_{y}}}$$
הPDF המשותף הוא מעין צורת פעמון כאשר המרכז הוא $\mu_{x},\mu_{y}$ . כמו כן באמצעות [קווי המתאר](https://www.khanacademy.org/math/multivariable-calculus/thinking-about-multivariable-function/ways-to-represent-multivariable-functions/a/contour-maps) כלומר בהינתן קבוע כלשהו קו מתאר מסויים יהיה מצב שבוא 

$$f_{X,Y}(x,y)=constant$$

במקרה שלנו נוכל להמיר את זה למצב שבו 

$${ \frac{(x-\mu_{x})^{2}}{2\sigma^{2}_{x}}+ \frac{(y-\mu_{y})^{2}}{2\sigma^{2}_{y}}}=\text{constant}$$

כי רק במעריך יש תלות במשתנים.

אם כן, נקבל שקווי המתאר האלה הם אליפסות במישור הדו מימדי ובמצב שבוא $\sigma_{y}=\sigma_{x}$ למשל אם שניהם היו סטנדרטים , היינו מקבלים מעגל.

![Pasted image 20230126115003.png|300](/img/user/Assets/Pasted%20image%2020230126115003.png)

![Pasted image 20230126115014.png](/img/user/Assets/Pasted%20image%2020230126115014.png)



