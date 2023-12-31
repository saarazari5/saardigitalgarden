---
{"dateCreated":"2023-07-26 14:05","tags":["advanced_programming2","web"],"pageDirection":"rtl","dg-publish":true,"permalink":"/computer-science/programming-concepts/react/","dgPassFrontmatter":true}
---

# React
React היא ספריית JavaScript לעיבוד ממשקי משתמש (UI). ממשק המשתמש בנוי מיחידות קטנות כמו לחצנים, טקסט ותמונות. React מאפשרת לך לשלב אותם לרכיבים הניתנים לשימוש חוזר, הניתנים לקינון.

לדוגמה:

```jsx
function Profile() {
  return (
    <img
      src="https://i.imgur.com/MK3eW3As.jpg"
      alt="Katherine Johnson"
    />
  );
}

export default function Gallery() {
  return (
    <section>
      <h1>Amazing scientists</h1>
      <Profile />
      <Profile />
      <Profile />
    </section>
  );
}
```

השאלה העיקרית שעולה כאן היא כיצד הכנסנו קוד שנראה כמו HTML  לתוך קומפוננטה שכתובה ב [[Computer Science/Programming Concepts/HTML - CSS - Javascript#JavaScript\|js]]. נדבר על זה בהמשך כי זה הליבה שעליה מבוסס React.

## export , import 
ניתן להצהיר על רכיבים רבים בקובץ אחד, אבל קבצים גדולים יכולים להיות קשים לניווט. כדי לפתור זאת, אפשר לייצא רכיב לקובץ משלו, ולאחר מכן לייבא את הרכיב הזה מקובץ אחר:


```jsx
export default function Gallery() {
  return (
    <section>
      <h1>Amazing scientists</h1>
      <Profile />
      <Profile />
      <Profile />
    </section>
  );
}
```

זאת דרך ייצוא נפוצה ב js , בשיטה זאת מתייחסים לקומפוננטות כפונקציות. ניתן להתייחס אליהן גם ב class אבל לא אתמקד בזה כאן.

## JSX
JSX היא החרבת סינתקס עבור JavaScript המאפשרת לך לכתוב סימון דמוי HTML בתוך קובץ JavaScript. למרות שיש דרכים אחרות לכתוב רכיבים, רוב מפתחי React מעדיפים את התמציתיות של JSX, ורוב בסיסי הקוד משתמשים בזה.

האינטרנט נבנה על HTML, CSS ו-JavaScript. במשך שנים רבות, מפתחי אתרים שמרו על תוכן ב-HTML, עיצוב ב-CSS והיגיון ב-JavaScript - לעתים קרובות בקבצים נפרדים! התוכן סומן בתוך HTML בעוד ההיגיון של הדף חי בנפרד ב-JavaScript: