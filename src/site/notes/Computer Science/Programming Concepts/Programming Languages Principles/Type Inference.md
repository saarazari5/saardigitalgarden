---
{"dateCreated":"2024-02-25 13:40","tags":["programming_language"],"pageDirection":"rtl","dg-publish":true,"permalink":"/computer-science/programming-concepts/programming-languages-principles/type-inference/","dgPassFrontmatter":true}
---

# Type inference
ב[[Computer Science/Programming Concepts/Programming Languages Principles/OCaml\|OCaml]] אחד הפיצרים שהקומפיילר נותן הוא type inference. הפיצ׳ר הזה מאפשר למתכנת לא להצהיר על המשתנה ישירות אלא לתת לקומפיילר באמצעות כללי היסק לפרש את הטיפוס בעצמו. 

נבין את כיצד עובד המנגנון של הסקת טיפוסים על מקרה מצומצם של השפה. נגדיר אותו כ $\lambda \text{OCaml}$, שילוב של [[Computer Science/Programming Concepts/Programming Languages Principles/Lambda calculus\|תחשיב למדא]] ואוקמל .בשפה זאת הביטויים מוגדרים כך 

1) כל משתנה הוא ביטוי.
2) אם x משתנה ו t ביטוי אז $\text{fun x}\to \text{t}$ ביטוי.
3) אם $t_{1},t_{2}$ ביטויים אז $t_{1}t_{2}$ ביטויים. 

__הגדרה:__ 
נניח קבוצה אינסופית של משתני טיפוס. 
	- אם $\alpha$ משתנה טיפוס אז הוא טיפוס.
	- אם T,S טיפוסים אז כך גם $T\to S$ .

__הגדרה:__
יחס ההטפסה $\Gamma\vdash t:T$ מוגדר כך -

$$\displaylines{
\text{O-VAR}: \dfrac{x:T\in \Gamma}{\Gamma\vdash x: T}\\ \\
\text{ O-APP} \dfrac{\Gamma\vdash t_{12}: T_{1}\to T_{2}  \ \ \Gamma\vdash t_{1}: T_{1}}{\Gamma\vdash t_{12}t_{1}: T_{2}} \\ \\
\text{ O-ABS} \dfrac{\Gamma\vdash x: T_{1} \ \ \Gamma\vdash t: T_{2}}{\Gamma\vdash (\text{fun x}\to t): T_{1}\to T_{2}}
}$$

## בעיית הסקת הטיפוסים
נגדיר את בעיה זו כך:
	_קלט-_ ביטוי t 
	_פלט-_ הקשר הטפסה $\Gamma$ ומיפוי m מביטויים שמופיעים ב t לטיפוסים כך שלכל תת-ביטוי $t'$ של t מתקיים $\Gamma\vdash t':m(t')$ (כולל t עצמו).

__פתרון:__
1) יצירת מערכת משוואות בין ביטוי לטיפוס
2) פתרון המערכת
3) תרגום הפתרון ל $\Gamma$ ו m המתאימים.

### יצירת מערכת משוואות
נגדיר t ביטוי נורמלי אם לכל 2 ביטויים $t_{1}=\text{ fun x...}$ ו $t_{2}=\text{ fun y...}$ שמופיעים ב t , המשתנים x ו y שונים זה מזה.

__נשים לב:__ לכל ביטוי יש ביטוי נורמלי השקול לו על ידי שימוש ב[[Computer Science/Programming Concepts/Programming Languages Principles/Lambda calculus#כלל אלפה\|כלל אלפה]] .

__הגדרה__ עבור t ביטוי נגדיר $A_{t}$ קבוצת המשוואות. לכל מופע של כל ביטוי שמופיע ב t ניצור משתנה טיפוס משלו ואז
1) אם $\alpha,\beta$ מתאימים למופעים שונים של אותו הטיפוס אז נוסיף את $\alpha =\beta$ ל $A_{t}$ .למשל עבור הפונקציה $fun \ x\to x$ נסמן עבור ה x הראשון שהוא מטיפוס $\alpha_{x}^{1}$ והשני הוא מטיפוס $\alpha_{x}^2$ ונוסיף את $\alpha_{x}^{1}=\alpha_{x}^{2}$ ל $A_{t}$ .
2) אם משתני הטיפוס $\alpha,\beta,\gamma$ מתאימים ל $t_{1},t_{2},t_{1}t_{2}$ בהתאמה אז נוסיף את $\alpha=\beta\to \gamma$ ל $A_{t}$.
3) לכל מופע $fun \ x\to t'$ אם $\alpha,\beta,\gamma$ מתאימים ל $x,t',fun \ x\to t'$ בהתאמה אז נוסיף את  $\gamma=\alpha\to \beta$ ל $A_{t}$.

__דוגמה:__ עבור הביטוי $t=(\text{ fun x}\to x)y$ 
$$\displaylines{
y\mapsto \alpha_{y}\\
x\mapsto y_{x}^{1}\\ x\mapsto \alpha_{x}^{2}\\
\\
\text{ fun x}\to \text{ x}\mapsto \alpha_{\text{fun x}\to \text{ x}}\\
t\mapsto \alpha_{t}
}$$

כעת נגדיר את $A_{t}$:
$$A_{T}= \{\alpha_{x}^{1}=\alpha_{x}^{2}   \ \  , \ \ \alpha_{\text{fun x}\to \text{ x}} = \alpha_{x}^{1}\to \alpha_{x}^{2}  \ \ , \ \ \alpha_{\text{fun x}\to \text{ x}}= \alpha_{y} \to \alpha_{t} \}$$
### תרגום הפתרון
_כיצד מתרגמים את הפתרון לכללי הטפסה ומיפוי?_ 
באמצעות כללי substitution. החלפה היא פונקציה שמתאימה טיפוס לכל משתנה טיפוס.
החלפה $\sigma$ מורחבת לטיפוסים באופן הבא

$$\sigma(T_{1} \to T_{2})= \sigma(T_{1})\to \sigma(T_{2})$$

__הגדרה:__ החלפה $\sigma$ מאחדת שיוויון $T_{1}=T_{2}$ אם $\sigma(T_{1})= \sigma(T_{2})$ .
$\sigma$ מאחדת קבוצת שוויונות אם היא מאחדת את כל השיוויונות בקבוצה.

נסתכל על הדוגמה ממקודם- 

$$\sigma (\alpha_{x}^{1})=\sigma(\alpha_{x}^{2})= \sigma(\alpha_{y})=\sigma(\alpha_{t})=\alpha$$
וגם 
$$\sigma(\alpha_{fun \ x\to x})= \alpha \to \alpha= \sigma(\alpha_{y})\to \sigma(\alpha_{t})= \sigma(\alpha_{y}\to \alpha_{t})=\sigma(\alpha_{x}^{1}\to \sigma_{x}^{2})$$

__הגדרה:__ יהי t ביטוי ו $\beta$ פונקציה שמתאימה לכל תת ביטוי $t'$ של t את משתנה הטיפוס המתאים לאחד המופעים של $t'$ ותהי $\alpha$ החלפה שמאחדת את $A_{t}$ אז 

$$\Gamma_{\alpha}^{\beta}= \{ x: \sigma(\beta(x)) \ \ | \ \ x\in \text{Vars(t)}\}$$


__משפט:__ $\Gamma_{\sigma}^{\beta}$ ו $\sigma \circ \beta$ הם הפתרון לבעיית ההטפסה של t. בצורה פורמלית: יהי $t$ ביטוי ו $\beta,\sigma$ כמו לעיל אז $\Gamma_{\sigma}^{\beta}\vdash t': \sigma(\beta(t'))$ 

אם אין $\sigma$ כנ״ל אז t לא מטופס היטב. 

### פתרון מערכת משוואות
__הגדרה__: 
בעיית היוניפיקציה-
	_קלט-_ קבוצה של שוויונות בין ביטויים
	_פלט-_ החלפה $\sigma$ שמאחדת אותם

למשל אם x=y נרצה לבנות $\sigma$ כך ש $\sigma(x)=\sigma(y)$ נוכל להגדיר ש $\sigma(x)=\sigma(y)=x$ .

ומה אם המשוואה אומרת ש $x+y=x \cdot y$ ? כיוון שלא משנה מה יהיו כללי ההחלפה של המשתנים יתקיים $\sigma(x+y)=\sigma(x)+\sigma(y)$ וכנ״ל עבור $\cdot$ אז אין פתרון. 

מקרה נוסף שיכול לעניין אותנו בניתוח האלגוריתם שנראה בהמשך הוא מהצורה 
$$f(t_{1},\dots, t_{n})=f(s_{1},\dots , s_{n})$$

נוכל למצוא $\sigma$ שתאחד כל $t_{i}, s_{i}$ בנפרד כלומר סיגמה שתקיים $\sigma(t_{i})=\sigma(s_{i})$.

והמקרה האחרון שיכול לעניין הוא $x=f(y)$ נוכל להגדיר ש $\sigma(x)= f(y)$ ו $\sigma(y)=y$ .

כעת נסתכל על האלגוריתם ב[[Computer Science/Programming Concepts/Programming Languages Principles/OCaml\|OCaml]]:
``` OCaml
type id = string;;
type term = | Var of id | Term of id * term list
(*invariant for substitution*)
(*no id on a lhs occurs in any term earlier in the list*)

type substitution = (id * term) list

(*check if a variable occurs in a term*)
let rec occurs (x : id)(t : term) = match t with 
	| Var y -> x = y
	| Term (_ , s)-> List.exists ( occurs x )s

(*substitute term s for all accurrences of variable x in term t*)
let rec subst (s : term)(x : id)(t : term) = match t with
	| Var y -> if x = y then s else t
	| Term (f,u) -> Term ( f, List.map (subst s x) u)


(*apply a substitution right to left *)
let apply (s: substitution)(t:term) = 
	List.fold_right (fun (x,u)->subst u x)s t

(*unidy one pair*)
let rec unify_one (s:term)(t:term) = match (s,t) with
	| (Var x, Var y) -> if x = y then [] else [(x,t)]
	| (Term(f,sc), Term(g,tc))-> if 
		f = g && List.length sc = List.length tc
			then unify(List.combine sc tc)
			else failwith "not unifiable: head symbol conflict"

	| ((Var x, Term(_,_ as t)))
	| (( Term(_,_ as t), Var x))->
		if occurs x t 
			then failwith "not unifiable: circularity"
			else [(x,t)]

(*unify a list of pairs*)
and unify (s: (term * term)list) = match s with
	| []->[]
	| (x,y) :: t ->
		let t2 = unify t in 
			let t1 = unify_one (apply t2 x)(apply t2 y) in t1@t2;;
```


נסתכל על דוגמה של ה ביטוי $\alpha \to \alpha$. לפי הקוד זה יהיה מהצורה `Term("->", [Var ('a'), Var('a')]`. 

* substitution הוא פונקצית ההחלפה $\sigma$ .
* occurs היא פונקציה שבודקת אם id כלשהו מופיע בביטוי t מסויים.
* subst - מחליפים את המופיעים החופשיים של x ב s בביטוי t
* apply - מפעילה את subst על כל איבר ב s מהסוף.
* unify_one and unify - נזכיר ש and זה keyword שמאפשר mutual recursion ב OCaml. זה נועד לפתור את המקרה של $f(t_{1},t_{2})=f(s_{1},s_{2})$ .
	* unify- מקבל רשימה של זוגות של term כאשר היחס בינהם הוא שיוויון. אם הרשימה ריקה אז המקרה ברור, אם יש איבר מהצורה (x,y) אז פותרים את יתר הרשימה t ואז מבצעים unify_one על התוצאה עם x ועל התוצאה עם y ומחזירים את שרשור התוצאה.
	* unify_one- מבצע unify על זוג של term יחיד שהיחס בינהם הוא שיוויון.
		* אם שניהם משתנים שונים אז מבצעים substitution לביטוי t.
		* אם המקרה יותר מורכב של פונקציות אז בודקים האם הם מכילים את אותו מספר ארגומנטים וגם שהם שווים ואז מבצעים unify לכל זוג בנפרד בצורה רקורסיבית
		* המקרה השלישי והאחרון בודק מצב שיש בצד אחד משתנה ובצד השני ביטוי ופשוט בודקים האם יש מצב שהמשתנה מופיע בביטוי ואם כן מחזירים שגיאה כי אחרת נקבל רקורסיה אין סופית של החלפות.

>[!note] 
>האלגוריתם הזה הוא לא היחיד שפותר את בעיית היוניפיקציה, האלגוריתם הזה מוצא את מה שנקרא Most General Unifier (MGU) וכל פתרון אחר אפשר למצוא באמצעות MGU . נאמר ש $\sigma$ היא יותר ״כללית״ מ $\sigma'$ אם קיימת $\sigma''$ כך ש $\sigma'=\sigma'' \circ \sigma$ 
>

