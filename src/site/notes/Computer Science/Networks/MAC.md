---
{"dateCreated":"2024-02-26 00:01","tags":["networks"],"pageDirection":"rtl","dg-publish":true,"permalink":"/computer-science/networks/mac/","dgPassFrontmatter":true}
---

# MAC
כתובת MAC היא כתובת המורכבת משישה צירופים של זוגות אותיות (A-F) ומספרים, המופרדים בינהם.
את ההפרדה אפשר לייצג במגוון דרכים, לדוגמה:
![Screenshot 2024-02-26 at 0.02.53.png|300](/img/user/Assets/Screenshot%202024-02-26%20at%200.02.53.png)

כתות MAC מחולקת לשני חלקים:

![Screenshot 2024-02-26 at 0.03.14.png|300](/img/user/Assets/Screenshot%202024-02-26%20at%200.03.14.png)

__למה צריך גם כתובת MAC וגם כתובת [[Computer Science/Networks/IP\|IP]]?__
לכל פקטה שנשלחת בין 2 מכשירים סמוכים מוצמדות שתי כתובות יעד, ה IP של מחשב היעד וMAC של הראוטר הבא בדרך אל היעד הסופי. כל ראוטר שמקבל פקטה משנה את כתובת הMAC לזו של הראוטר הבא אחריה, אך הוא לא נוגע בכתובת ה IP של היעד הסופי. זה ממשיך עד שהפקטה מגיעה לכתובת MAC של כרטיס הרשת שמגלה כי כתובת ה IP של היעד שייכת לו.

![Screenshot 2024-02-26 at 0.10.30.png|300](/img/user/Assets/Screenshot%202024-02-26%20at%200.10.30.png)


