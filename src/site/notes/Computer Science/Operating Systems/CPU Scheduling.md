---
{"dateCreated":"2023-09-04 11:16","tags":["operating_system","computer_science"],"pageDirection":"rtl","dg-publish":true,"permalink":"/computer-science/operating-systems/cpu-scheduling/","dgPassFrontmatter":true}
---



# CPU Scheduling

__Multiprogramming__: היא תכונה של מערכת ההפעלה שמאפשרת לנו להריץ יותר מ process אחד בכל פעם ובכך להגדיר את ניצולת המערכת על ידי החלפת processes שחלקם הם __IO-Bound__ (תהליכים שמבצעים רוב הזמן פעולות I/O) והאחרים הם __CPU-Bound__ (תהליכים שמצבעים רוב הזמן חישובי מעבד). כדי שמערכת הפעלה תוכל לקיים את התכונה הזאת עליה לבנות מנגנון תזמון שינהל את התהליכים השונים.

דיברנו על [[Computer Science/Operating Systems/Process Management#Types of Schedulers\|סוגי המתזמנים]] השונים שיש למערכת ההפעלה, ה Long Term וה Short Term ועל תפקידהם. כעת נבין את השיטות שהShort Term קובע איזה מן התהליכים הוא רוצה להריץ בכל רגע נתון.

## Short Term Scheduling
הוא רץ לפחות כאשר:
1. תהליך משנה את מצבו מ running ל waiting.
2. כאשר היה interrupt. 
3. תהליך נוצר או מת.


