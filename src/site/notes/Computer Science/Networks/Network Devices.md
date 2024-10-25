---
{"dateCreated":"2024-01-18 18:04","tags":["networks"],"pageDirection":"rtl","dg-publish":true,"permalink":"/computer-science/networks/network-devices/","dgPassFrontmatter":true}
---

# רכיבי תקשורת 
## Hub
Hub משמש בדרך כלל לחיבור מקטעים של LAN (רשת מקומית). הוא מכיל מספר Ports. כאשר פקטה מגיעה ליציאה אחת, היא מועתקת לפורטים האחרים כך שכל מקטעי ה-LAN יכולים לראות את כל החבילות. Hub פועל כנקודת חיבור משותפת למכשירים ברשת. המשמעות היא שבאחריות כל מכשיר המחובר ב Hub לבדוק האם הפקטה מיועדת אליו או לא.

![Screenshot 2024-01-18 at 18.34.53.png|250](/img/user/Assets/Screenshot%202024-01-18%20at%2018.34.53.png)
## Switch
switch פועל ב[[Computer Science/Networks/Computer Networks Intro and Protocol layers#Link Layer\|שכבת הלינק]] (שכבה 2) ולעיתים [[Computer Science/Networks/Computer Networks Intro and Protocol layers#Network Layer\|בשכבת הרשת]] (שכבה 3) של OSI ולכן תומך בכל פרוטוקול פקטות. רשתות LAN שמשתמשות Switch כדי להצטרף למקטעים נקראות Switched LAN. רכיב זה הוא רכיב שמנתב את הפקטות בין מקטעי ה LAN רק ליעד המתאים. 

ה Switch עושה זאת באמצעות Switch table שבו מוצמד לכל פורט [כתובת MAC](https://en.wikipedia.org/wiki/MAC_address)של המכשיר שמחובר אליה. 

![Screenshot 2024-01-18 at 18.36.06.png|250](/img/user/Assets/Screenshot%202024-01-18%20at%2018.36.06.png)


>[!info] הבחנה
>Hub ו Switches מאפשרים להעביר מידע בטווח של LAN. הם לא מאפשרים להוציא מידע מחוץ לרשת המקומית, כיוון שאין להם את היכולת לקרוא כתובות IP 


### Switch Table
כאשר הSwitch מקבל Frame , הFrame מכיל בHeaders שלו את הSource MAC ואת ה Dest Mac. ראשית הוא יודע לשייך את הSource MAC לפורט שממנו הגיע ה Frame, הוא מיד שומר זאת בטבלה  (הטבלה משמשת בCache ולכן המידע לא נשמר באופן תמידי). כעת מצב הטבלה הוא 

| MAC | Interface Number |
| --- | ---- |
| A    | 1     |
כאשר הSwitch לא יודע מיהו הDest Mac (אם הוא יודע כלומר זה שמור לו בטבלה הוא מיד מעביר את הframe ליעד), הוא שולח את ה frame לכל הנקודות חיבור חוץ מ IN=1 שזה הפורט של מי ששלח את הFrame, בשיטה שנקראת Broadcasting. רק המחשב שאמור לקבל את ההודעה מגיב בחזרה בFrame תגובה. כאשר מתקבל הFrame הזה ב Switch, הוא בודק את ה Source MAC שוב פעם ומעדכן שוב את הטבלה. כעת הטבלה נראת ככה: 

| MAC | IN |
| ---- | ---- |
| A | 1 |
| B | 2 |
## Router
ראוטר הוא מחובר לפחות לשתי רשתות, או שתי LANs או WANs (Wide Area Network). או LAN אל מול ה ISP (Internet Service Provider) .
הראוטר מנתב את מידע מרשת אחת לאחרת בהתאם לכתובת הIP שלהם.

הראוטר מקבל מפקטה את כתובת הIP והוא קובע האם זה מיועד לרשת שלו או שזה מיועד לרשת אחרת. בעצם הראוטר מהווה Gateway של הרשת המקומית.

![Screenshot 2024-01-18 at 18.31.06.png|400](/img/user/Assets/Screenshot%202024-01-18%20at%2018.31.06.png)

## ההבדלים
![Pasted image 20240118183746.png|400](/img/user/Assets/Pasted%20image%2020240118183746.png)
