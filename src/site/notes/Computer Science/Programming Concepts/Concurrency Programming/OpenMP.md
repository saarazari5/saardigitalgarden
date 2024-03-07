---
{"dateCreated":"2024-02-22 14:51","tags":["concurrency"],"pageDirection":"rtl","dg-publish":true,"permalink":"/computer-science/programming-concepts/concurrency-programming/open-mp/","dgPassFrontmatter":true}
---

# OpenMP
OpenMP API הוא אוסף כלים שמאפשרים תכנות מקבילי בשפת c ו cpp ושפות נוספות.
[[pthreads\|pthreads]] הוא עדיין הכלי העדיף מבחינת ביצועים וגמישות. עם זאת, מבחינת רמת האבסטרקצייה והנוחות למתכנת עם ביצועים טובים עבור הרבה תבניות עיצוב מקביליות. בכל מקרה, ניתן לכתוב קוד שמשלב בין השניים. 

OpenMP מתלבשת על כמה שכבות ב software stack, מרכיבים בסיסיים של OpenMP הם compiler directives, OpenMP library ו- environment variables.

![Pasted image 20240222150516.png|350](/img/user/Assets/Pasted%20image%2020240222150516.png)

נסתכל על קוד hello world לדוגמה ב OpenMP:

```C
#include <omp.h>
#include <stdio.h>

int main() {
	#pragma omp parallel
		{
			printf(" hello ");
			printf(" world\n ");
		}
	return 0;
}
```

השורה `pragma omp parallel` היא הנחיות לקומפיילר. 

הבלוק בפנים חייב להיות עם שורת כניסה אחת ושורת יציאה אחת. הבלוק שבפנים נקרא parallel region.

נבחן את פלט התוכנית:
![Pasted image 20240222151243.png](/img/user/Assets/Pasted%20image%2020240222151243.png)ראשית, נשים לב שקימפלנו עם הדגל fopenmp.
זה דגל שאומר לgcc להתייחס לdirectives שהגדרנו בקוד (במקרה הזה השורה pragma omp parallel). 

נתנו הוראה ל- gcc להריץ את הקוד שב- block במקביל עם מספר default של threads, ששווה (בדרך כלל) למספר cores שיש במחשב. לכן הקוד מתבצע במקביל ע"י 4 threads ואנחנו רואים 4 הדפסות של hello world.

>[!info] CPU Info
>כדי לקבל מידע מפורט על ארכיטקטורת המחשב שברשותנו נוכל להשתמש בפקודת lscpu. אם נרצה רק את מספר הליבות נשתמש בפקודת nproc 
>![Pasted image 20240222172229.png](/img/user/Assets/Pasted%20image%2020240222172229.png)
>

__num of threads:__
כדי להגדיר כמה threads נרצה כדי להריץ קטע קוד מקבילי נשתמש בפקודה omp_set_num_threads(n) לפני פתיחת הבלוק.

כדי לקבל את המזהה של כל thread נריץ את הפקודה omp_get_thread_num .