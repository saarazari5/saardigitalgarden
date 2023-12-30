---
{"dateCreated":"2023-03-01 15:58","tags":["abstract_algebra","computer_science"],"pageDirection":"rtl","dg-publish":true,"permalink":"/computer-science/algebraic-structure/noether-s-isomorphism-theorems/","dgPassFrontmatter":true}
---


# משפטי האיזומורפיזמים של נתר
שלושת משפטי ה[[Computer Science/Algebraic Structure/Cayley's And Lagrange theorem#הומומורפיזם\|איזומורפיזמים]] של נתר לחבורות הם משפטים יסודיים המקשרים בין הומומורפיזמים, חבורות מנה ותת־חבורות נורמליות. יש משפטים דומים למבנים אלגבריים אחרים, כולל הכללות בתחום של אלגברה אוניברסלית.נעסוק רק במשפט האיזומורפיזמים הראשון, שהוא העיקרי והשימושי מבין משפטי האיזומורפיזמים (את האחרים מוכיחים בעזרתו). למעשה, __הוא כה שימושי שכאשר נרצה להוכיח איזומורפיזם בין חבורת מנה לחבורה אחרת, כמעט תמיד נשתמש בו.__ 
נרצה לדבר על כל אלה לפני שנגדיר את המשפט עצמו.

## חבורה נורמלית
__הגדרה__: תהי $G$ חבורה ותהיינה $A,B\subseteq G$ נגדיר 

$$A\cdot B=\{ab \ | \ a\in A, b\in B\}$$

המקרה הפרטי הוא על $G$ [[Computer Science/Algebraic Structure/groups\|חבורה]] ו $H\leq G$ . ה[[Computer Science/Algebraic Structure/Cayley's And Lagrange theorem#מחלקה\|מחלקות]] $aH,bH$ יקיימו 

$$(aH)(bH)=\{xy  \ | \ x\in aH, y\in bH\}$$
==תת חבורה נורמלית== היא תת חבורה $H\leq G$ המקיימת 

$$\forall_{a\in G}: aH=Ha$$

נסמן תת חבורה נורמלית $H$ של $G$ על ידי $H\triangleleft G$ .

__משפט:__ עבור $H\triangleleft G$ מתקיים 

$$\forall_{a,b\in G}: (aH)(bH)= (ab)H$$

__תכונות של תת חבורה נורמלית שקולות__ 
1) $gHg^{-1}=H$ 
2) $gHg^{-1}\subseteq H$ 
3) $ghg^{-1}\in H$

__משפט__: אם $G$ אבלית ו $H\leq G$ אזי $H$ נורמלית.

## חבורת המנה 
קבוצת המנה היא אוסף המחלקות השמאליות $G/H$ כאשר $H\triangleleft G$ . 
__חבורת המנה__ מוגדרת להיות $(G/H,*)$ כאשר הפעולה שהגדרנו היא $(aH)(bH)$ 


>[!note] הבחנה
>נשים לב ש $|G/H|=[G:H]=\frac{|G|}{|H|}$ 

__משפט__ יהי $f:G\to H$ הומומורפיזם אזי $\ker(f)\triangleleft G$ .

## משפט האיזומורפיזם הראשון 
יהי $f:G\to H$ [[Computer Science/Algebraic Structure/Cayley's And Lagrange theorem#הומומורפיזם\|הומומורפיזם]] אזי $G/\ker(f)\cong{Im(f)}$ . 

__מסקנות:__
1) אם $f$ על אז $G/\ker(f)\cong{H}$.
2) אם $f$ חח״ע אז $G\cong Im(f)$.
3) אם $f$ הפיכה אז $G\cong H$ .

