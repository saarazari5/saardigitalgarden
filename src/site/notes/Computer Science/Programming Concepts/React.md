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

![Pasted image 20230726154016.png](/img/user/Assets/Pasted%20image%2020230726154016.png)

אבל ככל שהרשת הפכה לאינטראקטיבית יותר, הלוגיקה קבעה יותר ויותר את התוכן.
JavaScript התחיל להיות אחראי על ה HTML וזו הסיבה שב-React, רינדור הלוגיקה והdesign חיים יחד באותו מקום - כקומפוננטה.

![Pasted image 20230726154313.png](/img/user/Assets/Pasted%20image%2020230726154313.png)

כל רכיב React הוא פונקציית JavaScript שעשויה להכיל markup כלשהו ש-React מרנדר לדפדפן. רכיבי React משתמשים בהרחבת סינתקס הנקראת JSX כדי לייצג את הסימון הזה.

>[!info] הבחנה
>JSX ו React הם שני דברים נפרדים שלרוב משתמשים בהם ביחד. עם זאת, ניתן להשתמש בכל אחד מהם בנפרד.

### HTML to JSX
נניח שיש לנו את הHTML הבא

```HTML
<h1>Hedy Lamarr's Todos</h1>
	<img  
		src="https://i.imgur.com/yXOvdOSs.jpg"  
		alt="Hedy Lamarr"  
		class="photo" >
	<ul>  
		<li>Invent new traffic lights  
		<li>Rehearse a movie scene  
		<li>Improve the spectrum technology  
	</ul>
```

נרצה לשים את הHTML הזה בקומפוננטה שלנו שנקרא `TodoList` 

```JS
export default function TodoList() {  
	return (  
	// ???  
	)  
}
```

אם פשוט נעתיק לתוך הקומפוננטה את ה html נקבל שגיאה 

![Pasted image 20230726163516.png](/img/user/Assets/Pasted%20image%2020230726163516.png)
![Pasted image 20230726163529.png](/img/user/Assets/Pasted%20image%2020230726163529.png)
זה נובע בגלל ש JSX היא טיפה יותר מחמירה ויש לה קצת יותר חוקים מ HTML.

### החוקים של JSX

1. החזר ש single root element- חייב לעטוף את כל התגיות בתגית אחת. עבור המקרה שלנו נוכל לעשות 

![Pasted image 20230726163713.png](/img/user/Assets/Pasted%20image%2020230726163713.png)
או

![Pasted image 20230726163724.png](/img/user/Assets/Pasted%20image%2020230726163724.png)
התגית הריקה נקראת `Fragment`

>[!info] למה זה חובה?
>JSX נראה כמו HTML, אבל מתחת למכסה המנוע הוא הופך לאובייקטי JavaScript פשוטים. לא ניתן להחזיר שני אובייקטים מפונקציה מבלי לעטוף אותם במערך. זה מסביר מדוע אתה גם לא יכול להחזיר שני תגי JSX מבלי לעטוף אותם בתג אחר או פרגמנט.

2. חובה לסגור את כל התגים- בניגוד ל HTML קלאסי שאמרנו שזה לא חובה עבור תגיות שסוגרות את עצמן כמו `<img>` ב JSX חייבים לסגור אותם ידנית על ידי `</` .
3. החלפת רוב המשתנים עם $-$ ב camelCase למשל `stroke-width` יש להחליף ב `strokeWitdh`. מקרה קצת חריג הוא `class` שכן זאת מילה שמורה בשפה javascript ולכן הוחלפה ב `className`.

### JSX Dynamic Rendering
באמצעות סוגריים מסולסלים `{}` ניתן להעביר פרמטרים של javascript למשל 

```jsx
export default function Avatar() {
 const avatar = 'https://i.imgur.com/7vQD0fPs.jpg';
 const description = 'Gregorio Y. Zara';
  return (
		 <img
	      className="avatar"
	      src={avatar}
	      alt={description}
		 />
	);
}
```

בגלל ש JSX היא בסך הכל הרחבת סינתקס של JS , נוכל להשתמש בה כדי לכתוב JS בתוכה, יתרון עצום על פני HTML למשל:

```jsx
    <h1>{name}'s To Do List</h1>
```

ואפילו נוכל לקרוא לפונקציות 

```jsx
    <h1>To Do List for {formatDate(today)}</h1>
```

>[!info] העברת אובייקטים בJSX
>בגלל שהסינתקס של סוגריים מסולסלות מייצג גם אובייקטים אם נרצה להעביר ב JSX אובייקט נצטרך לפתוח שתי סוגריים למשל 
>![Pasted image 20230726170227.png|400](/img/user/Assets/Pasted%20image%2020230726170227.png)
>על הדרך גם נראה שניתן להעביר style כאובייקט ב JSX בניגוד ל HTML

### JSX Transform
דפדפנים לא מבינים את JSX בדיפולט, אז רוב משתמשי React מסתמכים על compiler כמו Babel או TypeScript כדי להפוך קוד JSX ל-JavaScript רגיל.
ערכות כלים רבות שהוגדרו מראש כמו Create React App או Next.js כוללות גם טרנספורמציה של JSX מתחת למכסה המנוע.

לא אתעמק בגרסאות חדשות יותר של JSX אבל בעבר כאשר המהדר של JSX היה נתקל בקוד JSX למשל 

``` jsx
  return <h1>Hello World</h1>;
```

ה JSX יבצע transform לקוד 

```jsx
return React.createElement('h1',      null, 'Hello world');
```

התהליך הזה נקרא Transpiration והיום הוא עובד קצת שונה אבל אין צורך להעמיק לצורך הסיכום הזה.

## Props
רכיבי React משתמשים ב props כדי לתקשר אחד עם השני. כל רכיב הורה יכול להעביר מידע מסוים לרכיבי הילד שלו על ידי מתן props. 
props עשויים להזכיר לך תכונות HTML, אבל אתה יכול להעביר דרכם כל ערך JavaScript, כולל אובייקטים, מערכים ופונקציות.

![Pasted image 20231231121416.png](/img/user/Assets/Pasted%20image%2020231231121416.png)

לשים לב שחייב את הסוגריים המסולסלות בהעברת ערך.

כדי לקרוא את ה props בקומפוננטת אוואטר נוכל פשוט לרשום את השמות של הערכים שהעברנו אותם באופן הבא 

``` JS
function Avatar({person, size})
```

אפשר גם להעביר באופן הבא 

```JS
function Avatar(props) {
	let person = props.person
	let size = props.size
}
```

בעצם props זה הפרמטר היחידים שקומפוננטה מקבלת והיא מכילה בתוכה עוד מידע שלרוב לא צריך, לכן הרבה פעמים מעבירים בצורה הראשונה ולא בשנייה.

אם מצהירים בצורה הראשונה אפשר גם לתת ערך דיפולטי

```JS
function Avatar({person, size = 100})
```

>[!info] destructing
>הסינתקס הזה שבוא מחליפים את הפרמטר props ב `{var1, var2}` נקרא destructing וזה שקול לקריאת הפרמטרים מהprops

### JSX Spread
לפעמים העברה יכולה לחזור על עצמה 
```jsx
function Profile({ person, size, isSepia, thickBorder }) {  
	return (  
<div className="card">  
	<Avatar 
	person={person}  
	size={size}  
	isSepia={isSepia}  
	thickBorder={thickBorder}  
	/>  
</div>  
);  
}
```

לפעמים נרצה לחסוך את הboiler plate הנ״ל ולשם כך נוכל להשתמש בסינתקס של JSX שנקרא spread. הוא נראה כך 
```jsx
function Profile(props) {
	return (
		<div className = "card">
			<Avatar {...props} />
		</div>	 
	)
}
```

### Passing JSX as children

כאשר מבצעים קינון של תגיות למשל 

```jsx
<Card>
	<Avatar />
</Card>
```

האבא Card מקבל את המידע הזה כחלק מהprops תחת השם `children`.

מה שקורה כאן זה שCard קיבל כפרמטר את הקומפוננטה Avatar ובהגדרת הקומפוננטה Card עלינו להגדיר כיצד צריך לרנדר אותה. 

כעת בקומפוננטה Card עצמה נבנה את המימוש 

```jsx
function Card({ children }) {
  return (
    <div className="card">
      {children}
    </div>
  );
}
```

![Pasted image 20230726201807.png](/img/user/Assets/Pasted%20image%2020230726201807.png)

### JSX conditions

* אפשר בReact  לשלוט בלוגיקת בראנציק עם javascript
* אפשר עם `if` להחזיר ביטוי JSX מסויים.
* ב JSX הביטוי $\{cond ? <A/> : <B />\}$ אומר : אם `cond` מתקיים אז תרנדר את A אחרת תרנדר את B.
*  הביטוי $\{cond \ \ \&\& <A/>\}$ אומר: אם `cond` מתקיים תרנדר את A אחרת כלום.

## Rendering lists
נניח שיש לנו רשימה של תוכן 
```HTML
<ul>  
 <li>Creola Katherine Johnson: mathematician</li>  
 <li>Mario José Molina-Pasquel Henríquez: chemist</li> 
 <li>Mohammad Abdus Salam: physicist</li>  
 <li>Percy Lavon Julian: chemist</li>  
 <li>Subrahmanyan Chandrasekhar: astrophysicist</li>  
</ul>
```

ההבדל היחיד בין אלמנטים ברשימה הוא המידע שהם מכילים בתוכם.
JSX הוא כלי טוב כדי להשתמש במבנה נתונים דינמי כמו רשימה ואובייקטים של JS , ביחד עם פונקציות על אוספים כמו `map,filter` כדי להמיר רשימות לקומפוננטות. 
עבור המערך הבא 

```jsx
const people = [  
'Creola Katherine Johnson: mathematician',  
'Mario José Molina-Pasquel Henríquez: chemist',  
'Mohammad Abdus Salam: physicist',  
'Percy Lavon Julian: chemist',  
'Subrahmanyan Chandrasekhar: astrophysicist'  
];
```

נוכל להמיר לקומפוננטה  כך

```jsx
export default function List() {
  const listItems = people.map(person =>
    <li>{person}</li>
  );
  return <ul>{listItems}</ul>;
}
```

נשים לב שנקבל את ההערה הבאה:

>[!warning] 
>Warning: Each child in a list should have a unique “key” prop.

כדי לטפל בשגיאה סך הכל צריך להוסיף פרמטר `key` לכל `li`
```jsx
<li key={person.id}>...</li>
```
חשוב לשים לב ש
1) על המפתחות להיות unique אם הם תחת אותה קומפוננטה
2) המפתחות צריכים להיות יחידים ולא להשתנות.

_נוכל גם לפלטר אלמנטים לפני שנציג אותם באופן הבא_
```jsx
const chemists = people.filter(person =>  
person.profession === 'chemist'  
);
```
ועל זה לעשות `map` כמו מקודם.

## טהרת הקומפוננטה
* קומפוננטה חייבת להיות __טהורה__ שזה אומר:

	1) הקומפוננטה לא יכולה לשנות אובייקט או משתנה שהיה קיים לפני הרינדור
	2) דטרמינסטית, עבור אותו קלט תמיד יתקיים אותו הפלט.

* רינדור יכול לקרות בכל זמן, קומפוננטות לא צריכות להיות תלויות ברינדור של האחרת.
* אסור לשנות שום קלט שהקומפוננטה משתמשת בו לרינדור, זה כולל props , states ו contexts. אם נרצה לעדכן את המסך נשתמש בפונקציות hooks עליהם נדבר בהמשך.
* ננסה לבטא את הלוגיקה של הקומפוננטה בתוך ה JSX שאנחנו מחזירים. אם נרצה לשנות ערכים נרצה לעשות זאת עם event handlers או במוצא אחרון להשתמש ב `useEffect` שגם עליו נדבר.
* זה בסדר לשנות משתנים שנוצרות תוך כדי הרינדור.

## Managing State
כמה דברים על המסך מתעדכנים בתגובה לקלט המשתמש. לדוגמה, לחיצה על גלריית תמונות משנה את התמונה הפעילה. ב-React, מידע שמשתנה עם הזמן נקרא State. אתה יכול להוסיף state לכל קומפוננטה, ולעדכן אותו לפי הצורך. 

### תגובה ל events
React מאפשרת הוספת handlers עם JSX. 
אומנם יש את הקומפוננטות המובנות שתומכות רק ב events מובנים אבל נוכל ליצור קומפוננטות משלנו וליצור להם events handlers ולהעביר אותם כ props. לקומפוננטות המובנות
```jsx

export default function App() {
  return (
    <Toolbar
      onPlayMovie={() => alert('Playing!')}
      onUploadImage={() => alert('Uploading!')}
    />
  );
}

function Toolbar({ onPlayMovie, onUploadImage }) {
  return (
    <div>
      <Button onClick={onPlayMovie}>
        Play Movie
      </Button>
      <Button onClick={onUploadImage}>
        Upload Image
      </Button>
    </div>
  );
}

function Button({ onClick, children }) {
  return (
    <button onClick={onClick}>
      {children}
    </button>
  );
}
```

נשים לב שאנחנו מעבירים את הפונקצייה עצמה כ props ולא מפעילים אותה

![Pasted image 20230727011537.png|350](/img/user/Assets/Pasted%20image%2020230727011537.png)

#### event propagation
event handlers יתפסו events מכל הילדים שהקומפוננטה שלך שהקומפוננטה עשויה להכיל. מונח זה נקרא event propagation , הוא גורם למאורעות לעלות למעלה בעץ הירכיית הקומפוננטות (זה נכון גם ל HTML ) .
למשל בקוד הבא אם נלחץ על הכפתור יופעל גם ה onclick של ה div

```jsx
export default function Toolbar() {
  return (
    <div className="Toolbar" onClick={() => {
      alert('You clicked on the toolbar!');
    }}>
      <button onClick={() => alert('Playing!')}>
        Play Movie
      </button>
      <button onClick={() => alert('Uploading!')}>
        Upload Image
      </button>
    </div>
  );
}
```

אם נרצה למנוע את זה נשתמש בפרמטר שעובר ב events ונפעיל עבורו את הפוקנצייה `event.stopPropagation`

>[!info] הבחנה
>למרות שאמרנו שקומפוננטות צריכות להיות טהורות, event handlers הם מקום טוב מאוד לשנות ערכים ומשתנים ששמורים בstate

### useState
רכיבים צריכים לעתים קרובות לשנות את מה שמופיע על המסך כתוצאה מאינטראקציה. הקלדה בטופס אמורה לעדכן את שדה הקלט, לחיצה על "הבא" בקרוסלת תמונה אמורה לשנות איזו תמונה מוצגת, לחיצה על "קנה" מכניסה מוצר לסל הקניות. רכיבים צריכים "לזכור" דברים: ערך הקלט הנוכחי, התמונה הנוכחית, עגלת הקניות.
ב-React, סוג זה של זיכרון ספציפי לרכיב נקרא state.

אפשר להוסיף state לקומפוננטה עם `useState` . זה בעצם חלק מסדרת פונקציות מיוחדות שנקראות Hooks שנותנות לקומפוננטות שלנו גישה לפיצ׳רים של React כאשר state הוא אחד מהם.

useState נותן לנו להגדיר משתנה state בהינתן מצב התחלתי קריאה לuseState תחזיר זוג ערכים, בstate הנוכחי ו state setter שאיתה נוכל לעדכן את ה state. 

```jsx
const [index, setIndex] = useState(0);  
const [showMore, setShowMore] = useState(false);
```

>[!info] הבחנה
>בעצם useState מחזירה מערך בגודל 2 ואנחנו עושים לו [destructing](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) שזה פיצ׳ר של השפה JS שמאפשר לנו להפריד את איברי המערך למשתנים בשורת קוד אחת.

הנה דוגמה לשימוש בuseStates שהגדנו
```jsx
import { useState } from 'react';
import { sculptureList } from './data.js';

export default function Gallery() {
  const [index, setIndex] = useState(0);
  const [showMore, setShowMore] = useState(false);
  const hasNext = index < sculptureList.length - 1;

  function handleNextClick() {
    if (hasNext) {
      setIndex(index + 1);
    } else {
      setIndex(0);
    }
  }

  function handleMoreClick() {
    setShowMore(!showMore);
  }

  let sculpture = sculptureList[index];
  return (
    <>
      <button onClick={handleNextClick}>
        Next
      </button>
      <h2>
        <i>{sculpture.name} </i>
        by {sculpture.artist}
      </h2>
      <h3>
        ({index + 1} of {sculptureList.length})
      </h3>
      <button onClick={handleMoreClick}>
        {showMore ? 'Hide' : 'Show'} details
      </button>
      {showMore && <p>{sculpture.description}</p>}
      <img
        src={sculpture.url}
        alt={sculpture.alt}
      />
    </>
  );
}
```

התוצאה נראת ככה: 

![Pasted image 20230727011051.png|400](/img/user/Assets/Pasted%20image%2020230727011051.png)

כאשר נלחץ על הshowMore נשנה את הstate וזה יוביל לרנדור מחדש והתנאי יגרום לכך שנציג את ה`<p>` שכתבנו

![Pasted image 20230727011307.png|400](/img/user/Assets/Pasted%20image%2020230727011307.png)
לחיצה על next תשנה את האלמנט עליו אנחנו מסתכלים במערך שהבאנו 

![Pasted image 20230727011337.png|400](/img/user/Assets/Pasted%20image%2020230727011337.png)

#### Render and commit
לפני שהרכיבים שלך יוצגו על המסך, הם חייבים להיות מעובדים על ידי React. הבנת השלבים בתהליך זה תעזור לנו לחשוב על אופן ביצוע הקוד שלך ולהסביר את התנהגותו.

דמיינו שהרכיבים שלכם הם טבחים במטבח, מרכיבים מנות טעימות מחומרים. בתרחיש זה, React הוא המלצר שמגיש בקשות מלקוחות ומביא להם את ההזמנות שלהם. לתהליך זה של בקשה והגשה של ממשק משתמש יש שלושה שלבים:

1. _triggering_ של רנדור - העברה של ההזמנה למטבח
2. _rendering_ של הקומפוננטה - הכנת המנה במטבח
3. _committing_ ל DOM - הגשה של המנה לשולחן

![Pasted image 20230727013232.png|400](/img/user/Assets/Pasted%20image%2020230727013232.png)
כאשר אפליקציית ה react שלנו נוצרת יש צורך לקרוא ל initial render. הרבה פעמים בframeworks השונים הקוד נכתב לבד או מוסתר מאיתנו אבל זה יקרה באופן הבא

```jsx
import Image from './Image.js';
import { createRoot } from 'react-dom/client';

const root = createRoot(document.getElementById('root'))
root.render(<Image />);
```

בחלק מהframeworks שמאתחלות פרוייקט React נוכל לראות גם קוד המשתמש ב `ReactDOM` ישירות למשל

```jsx
ReactDOM.render(
		<MainComponent/>,
		document.getElementById('root')
)
```
הרעיון הוא אותו דבר בעצם פשוט אנחנו משתמשים ישירות ב ReactDOM במקום לגשת ישירות לפונקציית createRoot שלו.
הפרמטר הראשון הוא הקומפוננטה והפרמטר השני הוא ה node ב DOM המקורי (תיכף נבדיל בין ReactDOM ל DOM) שנרצה לטעון אליו את הקומפוננטה.

לפני שReact נוצרה מפתחי web היו מבצעים מניפולציות על ה DOM ישירות באמצעות javascript או ספריות כמו [[Computer Science/Programming Concepts/HTML - CSS - Javascript#jquery\|jquery]] . התהליך היה יכול להיות מייגע ואיטי ולכן מפתחי React יצרו את ה virtual DOM ואת ה ReactDOM API. הראשון הוא עותק של ה DOM המקורי שמנוהל בצורה יותר יעילה ולאחר מכן משקף את התוצר על ה DOM המקורי. 
השני הוא API שמאפשר לנו לתווך בין ה DOM ל גרסה הוירטואלית שלו בצורה שנוחה עבור המפתחים עם כמה פונקציות API פשוטות.

__מה קורה ברינדור של קומפוננטה__
1) במהלך הרנדור הראשוני React ייצרו את ה DOM nodes הרלוונטים.
2) במהלך כל רנדור מחדש React תחשב אילו מהפרמטרים השתנו אם בכלל מאז הרנדור הקודם.

__מה קורה בקומיט של קומפוננטה לDOM__
לאחר הרינדור למסך React תבצע מניפולציות על הDOM.
* ברינדור ההתחלתי React תשתמש ב `appendChild` כדי להוסיף את כל הקודקודים הרלוונטים.
* עבור כל רינדור מחדש React תעדכן רק אם היו שינוי מידע בין הרנדורים.

#### State as a snapshot
בניגוד למשתנים רגילים בגאווה סקריפט, React state מתנהג יותר כמו snapshot. 
שינוי שלו באמצעות פונקציית set , לא משנה את הערך ישירות , במקום זה היא מריצה את הרנדור מחדש. 

```jsx
console.log(count); // 0  
setCount(count + 1); // Request a re-render with 1  
console.log(count); // Still 0!
```

רק לאחר הרנדור מחדש המידע נשמר שכן ה`useState` נקרא מחדש בעת רנדור הקומפוננטה והערך ההתחלתי כעת יהיה הערך החדש.

#### סדרה של עדכוני state
ההתנהגות שתיארתי למעלה יכולה לגרום להתנהגות לא צפויה כאשר קוראים לשינוי state מספר פעמים ברצף.

```jsx
import { useState } from 'react';

export default function Counter() {
  const [score, setScore] = useState(0);

  function increment() {
    setScore(score + 1);
  }

  return (
    <>
      <button onClick={() => increment()}>+1</button>
      <button onClick={() => {
        increment();
        increment();
        increment();
      }}>+3</button>
      <h1>Score: {score}</h1>
    </>
  )
}
```
בקומפוננטה הזאת לחיצה על הכפתור $+3$ תעדכן את התוצאה רק פעם אחת.

הסיבה לכך היא ששינוי state הוא לא פעולה מידית ובעצם יש בקשה לרינדור מחדש מReact כפי שכבר ציינו.
רינדור זה בעצם React מפעילה את הפונקצייה שהיא הקומפוננטה שלך וה jsx שחוזר ממנה מהווה snapshot של הUI כאשר כל המידע שמתלווה אליו מחושב בהתאם ל state ברגע הרינדור.

בניגוד להרבה snapshots אחרים, snapshot של UI הוא אינטרקטיבי. הוא כולל בתוכו לוגיקה והאזנה למאורעות כדי לתאר מה קורה כאשר המשתמש מבצע דברים מסויימים.

כאשר React מבצעת רינדור מחדש:
1. React תפעיל שוב את הפונקצייה שנקראת הקומפוננטה
2. נחזיר ממנה JSX חדש
3. נעדכן שוב את המסך ואת ה DOM.

![Pasted image 20230727020457.png|400](/img/user/Assets/Pasted%20image%2020230727020457.png)

בניגוד למשתנים מקומיים רגילים שנעלמים כי הם חיים רק ב function scope של הקומפוננטה, ה state נשמר על ידי React עצמה במאגר משלה, מחוץ לפונקצייה. כאשר React מפעילה את הפונקציה היא מתחשבת בערך החדש של ה state שנשמר על ידי react.

![Pasted image 20230727020655.png|400](/img/user/Assets/Pasted%20image%2020230727020655.png)

במילים אחרות שינוי state מכניס בקשה של רינדור מחדש עם הערך החדש של ה state הנוכחי עבור הקומפוננטה לתור של רינדורים מחדש.

__ולכן:__ כאשר בקוד הנ״ל קראנו לsetScore 3 פעמים ברציפות בעצם העברנו את __אותו ה snapshot__ לפני שהספיק להתבצע בכלל רינדור מחדש ולכן הערך יגדל רק ב $1$.

זה שקול ללבצע את הקוד 

```jsx
<button onClick={() => {  
	setNumber(0 + 1);  
	setNumber(0 + 1);  
	setNumber(0 + 1);  
}}>+3</button>
```

דוגמה קלאסית לזה היא שימוש ב `setTimeout` עם פרמטר שמוחזר מstate

```jsx
import { useState } from 'react';

export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button onClick={() => {
        setNumber(number + 5);
        setTimeout(() => {
          alert(number);
        }, 3000);
      }}>+5</button>
    </>
  )
}
```

במצב זה לחיצה על הכפתור תעשה 2 דברים: היא תשלח בקשה לרינדור מחדש עם הערך num+5 אבל תכניס לאחר 3 שניות בקשה ל message queue עם הערך שהיה לפני הרינדור שהוא 0 ולכן התוצאה תהיה

![Pasted image 20230727021605.png|400](/img/user/Assets/Pasted%20image%2020230727021605.png)

![Pasted image 20230727021618.png|300](/img/user/Assets/Pasted%20image%2020230727021618.png)

>[!info] חשוב לזכור
>בגלל איך ש Javascript עובדת, React מחכה עד שכל הקוד שבתוך ה event handler יסתיים לפני שהיא מבצעת את הרינדור מחדש ולכן הרינדור מחדש קורה רק אחרי שכל ה setState שלנו הופעל.
>כמו מלצר שמחכה להזמנה מכל השולחן לפני שהוא הולך 
>![Pasted image 20230727021843.png|400](/img/user/Assets/Pasted%20image%2020230727021843.png)

כדי לפתור את הבעיה הנ״ל אפשר להעביר ל setState פונקצייה שמרנדרת את הstate החדש לפי קודמו למשל:

```JS
setNumber(n => n+1)
```

בעצם לא העברנו את הnumber המקומי שאנחנו משתמשים בו ב function scope אלא הבאנו לReact פונקצייה כיצד לחשב את הערך הבא עם ה snapshot ששמור לה במדף.

אם נקרא לזה 3 פעמים התור ייראה ככה:

![Pasted image 20230727022200.png|300](/img/user/Assets/Pasted%20image%2020230727022200.png)

#### שינוי אובייקטים בuseState
כמובן שאפשר גם לייצר state על אובייקט 

```jsx
  const [person, setPerson] = useState({
    firstName: 'Barbara',
    lastName: 'Hepworth',
    email: 'bhepworth@sculpture.com'
  });
```

שינוי של האובייקט ישירות למשל `preson.firstName = saar` לא יגרום לרינדור מחדש. 

נוכל אם כן לשנות את האובייקט בכמה דרכים
1. ==(לא קונבנצייה טובה!)== לבצע mutate לאובייקט כמו שעשינו למעלה ואז לקרוא לsetState .
2. לייצר אובייקט חדש עם ערכים שונים (אולי חלקם יהיו זהים) ולקרוא ל setState עליו.
3. לבצע spread על האובייקט ולדרוס את מה שנרצה
```jsx
setPerson({  
	...person, // Copy the old fields  
	firstName: e.target.value // But override this one 
});
```
השיטה של spread תעבוד גם אם יש nested objects למשל:

```jsx
const [person, setPerson] = useState({  
name: 'Niki de Saint Phalle',  
	artwork: {  
	 title: 'Blue Nana',  
	 city: 'Hamburg',  
	 image: 'https://i.imgur.com/Sd1AgUOm.jpg',  
	}  
})

const nextArtwork = { ...person.artwork, city: 'New Delhi' };  
const nextPerson = { ...person, artwork: nextArtwork };  
setPerson(nextPerson);
```

>[!info] מערכים
>באופן דומה נוכל לבצע spread למערכים ולשנות ערכים בו או להוסיף אליו אלמנטים חדשים 

```jsx
setArtists( // Replace the state  
	[ // with a new array  
	 ...artists, // that contains all the old items  
	 { id: nextId++, name: name } // and one new item        at the end  
	]  
)
```

![Pasted image 20230727023324.png|500](/img/user/Assets/Pasted%20image%2020230727023324.png)

#### שיתוף State בין קומפוננטות
הפתרון הפשוט ביותר הוא פתרון שנקרא __lift up state__ שאומר להעביר את הstate מקומפוננטת האב דרך הילדים באמצעות props.

```jsx
import { useState } from 'react';

export default function Accordion() {
  const [activeIndex, setActiveIndex] = useState(0);
  return (
    <>
      <h2>Almaty, Kazakhstan</h2>
      <Panel
        title="About"
        isActive={activeIndex === 0}
        onShow={() => setActiveIndex(0)}
      >
        With a population of about 2 million, Almaty is Kazakhstan's largest city. From 1929 to 1997, it was its capital city.
      </Panel>
      <Panel
        title="Etymology"
        isActive={activeIndex === 1}
        onShow={() => setActiveIndex(1)}
      >
        The name comes from <span lang="kk-KZ">алма</span>, the Kazakh word for "apple" and is often translated as "full of apples". In fact, the region surrounding Almaty is thought to be the ancestral home of the apple, and the wild <i lang="la">Malus sieversii</i> is considered a likely candidate for the ancestor of the modern domestic apple.
      </Panel>
    </>
  );
}

function Panel({
  title,
  children,
  isActive,
  onShow
}) {
  return (
    <section className="panel">
      <h3>{title}</h3>
      {isActive ? (
        <p>{children}</p>
      ) : (
        <button onClick={onShow}>
          Show
        </button>
      )}
    </section>
  );
}
```

#### שמירה וreset של State
state מבודד בין הרכיבים. React עוקב אחר איזה state שייך לאיזה קומפוננטה על סמך מקומו בעץ ה-UI. נוכל לשלוט מתי לשמר את המצב ומתי לאפס אותו בין רינדור מחדש.

React מייצרת עץ מה JSX השונים ומעדכנת את ה DOM לאחר מכן

![Pasted image 20230727024451.png|400](/img/user/Assets/Pasted%20image%2020230727024451.png)

state , כפי שכבר ניתן היה להבין מההסברים הקודמים, לא חי בתוך הקומפוננטה אלא נשמר אצל React פר קומפוננטה.
לכן למשל הקוד הבא

```jsx
import { useState } from 'react';

export default function App() {
  const counter = <Counter />;
  return (
    <div>
      {counter}
      {counter}
    </div>
  );
}

function Counter() {
  const [score, setScore] = useState(0);
  const [hover, setHover] = useState(false);

  let className = 'counter';
  if (hover) {
    className += ' hover';
  }

  return (
    <div
      className={className}
      onPointerEnter={() => setHover(true)}
      onPointerLeave={() => setHover(false)}
    >
      <h1>{score}</h1>
      <button onClick={() => setScore(score + 1)}>
        Add one
      </button>
    </div>
  );
}
```
מחזיק 2 states שונים למרות ששמנו JSX אחד תחת const
![Pasted image 20230727024724.png|300](/img/user/Assets/Pasted%20image%2020230727024724.png)

העץ ייראה ככה: 
![Pasted image 20230727024745.png|300](/img/user/Assets/Pasted%20image%2020230727024745.png)

אם כן , כעת כשאנחנו יודעים איך המנגנון עובד, נרשום מספר כללי אצבע לכל הקשור לשמירה ואתחול מחדש של ה state.

1) state נצמד למיקום של קומפוננטה בעץ.
2) הסרה של הקומפוננטה והוספתה מחדש לעץ, תאתחל את הstate.
3) שמירה של הקומפוננטה באותו מיקום בעץ , תשמר את הstate (גם אם יש לה כמה state ואחד מהם השתנה האחרים עדיין נשארים אותו דבר כי המיקום בעץ לא השתנה.)
4) החלפה של קומפוננטות במיקום כלשהו בעץ, תאפס את הstate גם אם מדובר באותה קומפוננטה 
__(למשל רינדור מחדש עם תנאי כלשהו ואם התנאי משתנה הקומפוננטה מתחלפת וחוזרת בחזרה מאוחר יותר)__
5) אתחול מחדש של הstate כאשר הוא נמצא באותו מיקום בעץ יכול להעשות או על ידי אופצייה 4 או באמצעות הפרמטר `key` וככה React יודעת להבדיל בין קומפוננטות באותו מיקום בעץ. אם היא מזהה שהמפתח השתנה , היא תרנדר מחדש. 

### Context
בדרך כלל נעביר מידע מקומפוננטת אבא לבן באמצעות props.
התהליך הזה טוב להרבה מקרים והוא נקרא state up.
אבל התרחיש שיכול להווצר מזה נקרא prop drilling שזה מצב שבוא מחלחלים את ה prop עמוק בתוך העץ עד שמשתמשים בו באמת.

![Pasted image 20230727030306.png|400](/img/user/Assets/Pasted%20image%2020230727030306.png)

קונטקסט הוא אלטרנטיבה טובה שפותרת את המצב הזה והיא טובה למצבים שבהם נרצה להעביר מידע כלשהו מהקומפוננטה הגבוהה ביותר ועד לנמוכות ביותר בעץ (למשל, user שמחובר כרגע לאפליקציה שלנו).

נרצה למשל להחליף מצב כזה 

```jsx
<Section>  
 <Heading level={3}>About</Heading>  
 <Heading level={3}>Photos</Heading>  
 <Heading level={3}>Videos</Heading>  
</Section>
```
במצב שבו מעבירים את הprop רק ל Section והילדים מצליחים להשתמש בו בלי לקבל אותו 

```jsx
<Section level={3}>  
 <Heading>About</Heading>  
 <Heading>Photos</Heading>  
 <Heading>Videos</Heading>  
</Section>
```
במימוש הנוכחי זה לא יעבוד אבל עם Context נוכל לגרום לזה לקרות.

#### יצירת context
יצירת קובץ חדש וייצור שלו החוצה.
```JS
import { createContext } from 'react';
export const LevelContext = createContext(1);
```
ה createContext מקבל ערך התחלתי של 1.

#### שימוש ב context
לייבא את הקונטקסט 
```JS
import { useContext } from 'react';  
import { LevelContext } from './LevelContext.js';
```

להוציא את ה `level` מה props של Heading ונקרא אותו כך 

```jsx
export default function Heading({ children }) {  
const level = useContext(LevelContext);  
// ...  
}
```

`useContext` זה hooks בידיוק כמו `useState` ולכן אפשר לקרוא לו רק בתוך הקומפוננטה.

==כעת נוכל לשלוף את הcontext אבל יהיה לו ערך התחלתי של 1 תמיד.== 

#### provide the context
כל שנשאר לעשות הוא שיהיה ניתן לשנות את הcontext ממקומות מסויימים לאורך הקומפוננטות שלנו בעץ זה נעשה עם provider 

```jsx
import { LevelContext } from './LevelContext.js';  

  

export default function Section({ level, children }) { 
return (  
	<section className="section">  
	<LevelContext.Provider value={level}>  
		{children}  
	</LevelContext.Provider>  
	</section>  
	);  
}
```

זה אומר ל React ש ״אם מישהו מנסה לגשת ל LevelContext בתוך ה Section, תספק לו את הערך של ה level המתקבל ב props.

וכעת המידע באמת יעבור לילדים בצורה נכונה 

```jsx
import Heading from './Heading.js';
import Section from './Section.js';

export default function Page() {
  return (
    <Section level={1}>
      <Heading>Title</Heading>
      <Section level={2}>
        <Heading>Heading</Heading>
        <Heading>Heading</Heading>
        <Heading>Heading</Heading>
        <Section level={3}>
          <Heading>Sub-heading</Heading>
          <Heading>Sub-heading</Heading>
          <Heading>Sub-heading</Heading>
          <Section level={4}>
            <Heading>Sub-sub-heading</Heading>
            <Heading>Sub-sub-heading</Heading>
            <Heading>Sub-sub-heading</Heading>
          </Section>
        </Section>
      </Section>
    </Section>
  );
}
```

![Pasted image 20230727031921.png|350](/img/user/Assets/Pasted%20image%2020230727031921.png)

### useRef
כאשר נרצה שקומפוננטה תזכור מידע מבלי לרנדר אותו מחדש נשתמש ב `useRef`

```JS
const ref = useRef(0)
```
ref הוא אובייקט מהצורה `{current:0}`. הערך הזה הוא mutable , וזה בכוונה ככה, React לא עוקבת אחריו כלומר זה מעין אובייקט ששמור לצורך שמירת מידע בזמן מחזור החיים של הקומפוננטה. כשהיא תרונדר מחדש לא נקבל את הערך האחרון שעבדנו איתו.

ישנם מספר הבדלים שחשוב לציין:
![Pasted image 20230727032135.png|400](/img/user/Assets/Pasted%20image%2020230727032135.png)

יש לuseRef מספר שימושים עיקריים: 
1. ניתן להעביר אותו עבור קומפוננטה כפרמטר `ref` ואז להפעיל לcurrent החוזר ממנה פונקצייה הנקראת `focus` 
```jsx
import { useRef } from 'react';

const MyInput = forwardRef((props, ref) => {
  return <input {...props} ref={ref} />;
});

export default function MyForm() {
  const inputRef = useRef(null);

  function handleClick() {
    inputRef.current.focus();
  }

  return (
    <>
      <MyInput ref={inputRef} />
      <button onClick={handleClick}>
        Focus the input
      </button>
    </>
  );
}

```

בשיטה הזאת נוכל להפעיל focus על אלמנט כלשהו ב DOM מבלי שהיוזר ייגע בו ישירות.

2. ניתן לעקוב אחרי שינויי מידע בין רינדורים על ידי שמירה של הstate הקודם בתוך ref.

### useEffect
כדי להבין כיצד להשתמש בהוק React useEffect, חשוב להבין תחילה מה הוא עושה. ה-useEffect Hook ב-React מאפשר לך לבצע תופעות לוואי ברכיבים פונקציונליים. תופעות הלוואי יכולות לכלול שליפת נתונים מ-API, הרשמה לאירועים, מניפולציה של ה-DOM ועוד.

כפי שאמרנו נשתמש בו כאשר לא נדע בידיוק מיהו הevent שאפשר לשייך למידע שלנו. המקרה הקלאסי הוא מצב שבוא נרצה לעשות משהו ברגע שהקומפוננטה נטענת למסך.

מסיבה זאת זה גם נקרא useEffect, כי אנחנו מצמידים למידע מסויים side effect שקורה ברגע שהוא משתנה.

כדי לייצר effect נלך על השלבים הבאים
__יצירת האפקט__
```JS
import { useEffect } from 'react';
```
לקרוא לו בתוך הקומפוננטה הרלוונטית

```jsx
function MyComponent() {  	
	useEffect(() => {  
	// Code here will run after *every* render  
	});  
	return <div />;  
}
```
במצב זה האפקט ייקרא לאחר שהקומפוננטה תתרנדר למסך (גם ברנדור התחלתי וגם בכל רנדור מחדש).

>[!error] שגיאה נפוצה
>בגלל שאפקטים נקראים לאחר כל רנדור , קוד כמו זה יגרום ללולאה אינסופית
>![Pasted image 20230727040205.png](/img/user/Assets/Pasted%20image%2020230727040205.png)

אם נרצה להפעיל את האפקט רק ברנדור ההתחלתי נוכל להעביר לאפקט שלנו מערך ריק למשל
```JS
useEffect(() => {
    // This will only run on initial render
    fetchData().then(response => setData(response));
  }, []);
```

החסרון בשיטה הזו היא שברגע שמשתמשים ב dependency בתוך האפקט נצטרך להצמיד אותו למערך למשל 

```JS
useEffect(() => {  
	if (isPlaying) { // It's used here...  
	// ...  
	} else {  
	// ...  
	}  
}, [isPlaying]); // ...so it must be declared here!
```

במצב זה נריץ את האפקט כל פעם שisPlaying משתנה בין רנדורים.

__cleanup function__ 
נוכל להוסיף לוגיקה שמבצעת ניקוי של משאבים מתוך האפקט לפני שהוא רץ מחדש ופעם אחרונה כשהקומפוננטה יוצאת מההירכייה 

```JS
useEffect(() => {  
	const connection = createConnection();  
	connection.connect();  
	return () => {  
	connection.disconnect();  
	};  

}, []);
```

## CSS ו React
כפי שכבר ראינו ניתן לשלב CSS ו React על ידי העברת פרמטר style עם אובייקט js כ prop.
ברוב המקרים זה לא יספיק לנו אבל נרצה לספק stylesheet לקומפוננטה שלנו. לשם כך נייצר קובץ css מתאים ופשוט נייבא אותו.

למשל נייצר קובץ שנקרא `App.css

```css
body {
  background-color: #282c34;
  color: white;
  padding: 40px;
  font-family: Arial;
  text-align: center;
}
```

ופשוט נייבא אותו היכן שנרצה 
```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import './App.css';

class MyHeader extends React.Component {
  render() {
    return (
      <div>
      <h1>Hello Style!</h1>
      <p>Add a little style!.</p>
      </div>
    );
   
```

>[!info] Assets
>טעינה של assets יעבוד באופן דומה לטעינה של קובץ css
>![Pasted image 20230727041801.png|350](/img/user/Assets/Pasted%20image%2020230727041801.png)


## React Route
ניווט בין דפים מבוסס URL במקום state.
```jsx
// App.js
import { Routes, Route } from 'react-router-dom';
import Home from './Pages/Home';
import About from './Pages/About';
import Products from './Pages/Products';
 
const App = () => {
   return (
      <>
         <Routes>
            <Route path="/" element={<Home />} />
            <Route path="/products" element={<Products />} />
            <Route path="/about" element={<About />} />
         </Routes>
      </>
   );
};
 
export default App;
```
תחילה היינו יוצרים את הרכיבים הללו, במקרה שלנו שלושה דפים: דף הבית, דף אודות ודף המוצרים. מאגר GitHub זה מכיל את התוכן של דפים אלה. לאחר שעמודים אלו מוגדרים כהלכה, נוכל כעת להגדיר ולהגדיר את המסלולים שלנו בקובץ App.js, המשמש כבסיס לאפליקציית React שלנו.

השימוש הראשון אם כן הוא שייוך באמצעות URL אבל נוכל לבצע שיוך באמצעות תפריט למשל 

```jsx
// App.js
import NavBar from './Components/NavBar';
 
const App = () => {
   return (
      <>
         <NavBar />
         <Routes>
            // ...
         </Routes>
      </>
   );
};
export default App;





// Components/NavBar.js
import { Link } from 'react-router-dom';
 
const NavBar = () => {
   return (
   <nav>
         <ul>
            <li>
               <Link to="/">Home</Link>
            </li>
            <li>
               <Link to="/about">About</Link>
            </li>
            <li>
               <Link to="/products">Products</Link>
            </li>
         </ul>
   </nav>
   );
};
```
