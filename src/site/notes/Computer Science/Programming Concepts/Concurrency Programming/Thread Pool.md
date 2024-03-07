---
{"dateCreated":"2024-02-19 04:32","tags":["concurrency"],"pageDirection":"rtl","dg-publish":true,"permalink":"/computer-science/programming-concepts/concurrency-programming/thread-pool/","dgPassFrontmatter":true}
---

# Thread Pool
הקונספט של thread pool הוא חלק משמעותי בכל הקשור לתכנות מקבילי, בעיקר כאשר רוצים להריץ מספר משימות בצורה מקבילית באותה תוכנה.

thread pool בעקרון הוא אוסף של threads שאותחלו מראש תחת הגבלות מסוימות שנמצאים במצב idle ומחכים לקבלת משימות. ישנו איזה מתזמן שאחראי על חלוקה של המשימות לתהליכים השונים שמחכים ודואג לנהל את המשאבים כדי שלא נחרוג מההגבלות שהוגדרו מראש.

##  Efficiency
יצירה והריסת תהליכים יכולה להיות פעולה מורכבת. thread pools עוזרים לנו עם טיפול בoverhead הזה על ידי מחזור threads בשביל מספר משימות. ברגע שמשימה הושלמה הthread ממתין למשימות נוספות.
## Concurrency Control
Thread pools מאפשרות ניהול של רמת המקביליות בתוכנית על ידי הגבלה של מספר הthreads שיכולים לרוץ במקביל. זה חשוב כדי למנוע תופעות כמו thrashing שבהן יש יותר מדי threads שמתחרים על זמן מעבד ומשאבי זכרון מה שגורם לירידה בביצועים. על ידי שינוי מספר הthreads ב thread pool מתכנתים יכולים למצוא את נקודת האיזון שתאפשר מיקסום ביצועים וניצול משאבים אופטימלי.

## Task Scheduling and Execution
Thread pools לרוב מגיעים עם task queues מובנים. כאשר משימה חדשה מתקבלת לpool וכל התהליכים עסוקים, המשימה נכנסת לתור. ברגע שתהליך נהיה זמין הוא אוסף את המשימה הבאה מהתור כך שנוכל לנהל מנגנון תעדוף משימות וביטול משימות בעת הצורך.

## Types of Thread Pools
ישנן מספר אסטרטגיות ליצירת thread pools וניהולן:
_fixed size:_ הבריכה תקבל גודל קבוע של threads. אם כל הthreads עסוקים, משימות נכנסות לתור עד אשר אחד מהם מתפנה.

_cached:_ בריכות מהסוג הזה ייצרו threads חדשים לפי הצורך אבל ימחזרו threads ישנים שנוצרו אם הם פנויים.

_work stealing:_ בריכות מסוג זה מאפשרות לthreads ״לגנוב״ משימות מ threads אחרים כאשר יש מספר תורים למספר threads.
## Implementation 
נבחן מימוש של cached thread pool ב c.

```C
struct ThreadPool {
    unsigned int maxThreads;
    atomic_int runningThreads;
    TaskQueue *q;
};

typedef struct ThreadPool ThreadPool;

void runThreadPool(ThreadPool *tp);
```

נגדיר struct ThreadPool עם הנתונים הבאים:
	א) המספר המקסימלי של threads.
	ב) משתנה אטומי שקובע כמה threads רצים ברגע נתון.
	ג) task queue

בפונקציה של `runThreadPool` נגע בהמשך. כעת נגדיר את TaskQueue שזה בעצם מימוש בסיסי של FIFO [[Computer Science/Data Structures/Linear Data Structures#תור\|Queue]]:

```C
struct TaskQueue {
    TaskDataNode *start;
    TaskDataNode *end;
    pthread_mutex_t mutex;
};

typedef struct TaskQueue TaskQueue;

void initQueue(TaskQueue *q);

void insert(TaskQueue *q, TaskData td);

TaskData pop(TaskQueue *q);

int isEmpty(TaskQueue *q);
```

כחלק מהמימוש של תור יש צורך להגדיר struct Node שיחזיר בתוכו Data כלשהו. לשם כך נוצרו struct TaskDataNode ו struct TaskData כאשר ה Data הוא המידע שthreads צריך כדי לרוץ.

```C
typedef struct TaskData TaskData;

struct TaskDataNode {
    TaskData data;
    struct TaskDataNode *next;
};

struct TaskData {
    void * (*function)(void *);
    void *args;
};
```

```C
#include "TaskQueue.h"
#include <stdlib.h>
#include <stdio.h>

void initQueue(TaskQueue *q) {
    /* Initialize the mutex */
    if (pthread_mutex_init(&(q->mutex), NULL) != 0) {
        perror("mutex init has failed.");
        exit(1);
    }

    /* Init start and end to NULL */
    q->end = q->start = NULL;
}

void insert(TaskQueue *q, TaskData td) {
    /* Create a new node with the given data */
    TaskDataNode *newNode = malloc(sizeof(TaskDataNode));
    newNode->next = NULL;
    newNode->data = td; 

    /* Synchronously insert the new node */
    pthread_mutex_lock(&q->mutex);

    if ((q->end == NULL && q->start != NULL) ||
        (q->end != NULL && q->start == NULL)) {
        perror("'end' and 'start' are not in sync.");
        exit(1);
    }

    if (q->end == NULL) {
        q->start = q->end = newNode;
    } else {
        q->end->next = newNode;
        q->end = q->end->next;
    }

    pthread_mutex_unlock(&q->mutex);
}

TaskData pop(TaskQueue *q) {
    /* Synchronously pop the first element */
    pthread_mutex_lock(&q->mutex);

    if ((q->end == NULL && q->start != NULL) ||
        (q->end != NULL && q->start == NULL)) {
        perror("'end' and 'start' are not in sync.");
        exit(1);
    }

    if (q->start == NULL) {
        perror("cannot pop from an empty queue.");
        exit(1);
    }

    TaskDataNode *popNode = q->start;

    if ((q->start = popNode->next) == NULL) {
        q->end = NULL;
    }

    pthread_mutex_unlock(&q->mutex);

    /* Free the popped node and return the data */
    TaskData output = popNode->data;
    free(popNode);
    return output;
}

int isEmpty(TaskQueue *q) {
    /* Synchronously check if the queue is empty */
    pthread_mutex_lock(&q->mutex);
    int isEmpty = (q->start == NULL);
    pthread_mutex_unlock(&q->mutex);

    /* Return the result */
    return isEmpty;
}

```
נשים לב שכל הפונקציות הן thread safe באמצעות שימוש במנעולים.

```C
void runThreadPool(ThreadPool *tp) {
    /* As long as there are tasks in the queue or running threads... */
    while (!isEmpty(tp->q) || tp->runningThreads > 0) {
        /* If we are not past max number of threads */
        if (tp->runningThreads < tp->maxThreads && !isEmpty(tp->q)) {
            /* Run task */
            TaskData data = pop(tp->q);

            pthread_t thread;
            if (pthread_create(&thread, NULL, data.function, data.args)) {
                perror("pthread_create failed.");
                exit(1);
            } else {
                ++(tp->runningThreads);
            }
        }
    }
}
```
במימוש שלנו אנחנו מגדירים שthread pool יחיה כל עוד כמות התליכים שרצים גדולה מ0 או שיש עוד משימות בתור


 נראה כיצד ניתן להשתמש בthread pool כדי לבנות מנגנון ל[[Computer Science/Algorithms/Parallel Algorithms/Parallel Graphs Algorithms#DFS\|Parallel DFS algorithm]]

```C
struct dfsArgs {
    Graph *graph;
    vertex v;
    ThreadPool *q;
};

typedef struct dfsArgs dfsArgs;

void * parallel_dfs_visit(void *args) {
    /* Extract arguments */
    dfsArgs *data = (dfsArgs *) args;
    Graph *graph = data->graph;
    vertex v = data->v;
    ThreadPool *q = data->q;
    free(data);

    /* Log the visit */
    printf("%d ", v);

    /* Start the visit */
    node *neighborsPtr = graph->adjacencyLists[v];

    while (neighborsPtr != NULL) {
        vertex neighbor = neighborsPtr->v;

        /* Synchronously increment the number of visits for the neighbor */
        pthread_mutex_lock(&graph->num_visits_mutexes[neighbor]);
        int neighborVisits = graph->num_visits[neighbor]++;
        pthread_mutex_unlock(&graph->num_visits_mutexes[neighbor]);

        /* If the neighbor has not been visited, add a new task to the queue */
        if (neighborVisits == 0) {
            dfsArgs *params = malloc(sizeof(dfsArgs));
            params->graph = graph;
            params->v = neighbor;
            params->q = q;
            TaskData td = {parallel_dfs_visit, params};

            insert(q->q, td);
        }

        neighborsPtr = neighborsPtr->next;
    }

    /* Decrement the number of running threads */
    --(q->runningThreads);
}

void parallel_dfs(Graph *graph) {
    int numVertices = graph->numVertices;
    dfsArgs *args;

    /* For each vertex, if it is not visited, perform DFS. */
    for (vertex v = 0; v < numVertices; v++) {
        if (graph->num_visits[v] == 0) {
            printf("\nDFS Traversal: ");

            /* Init thread pool */
            TaskQueue q;
            initQueue(&q);

            ThreadPool pool;
            pool.maxThreads = 4;
            pool.runningThreads = 0;
            pool.q = &q;

            /* Insert the root task to the queue */    
            args = malloc(sizeof(dfsArgs));
            args->graph = graph;
            args->v = v;
            args->q = &pool;

            //pthread_mutex_lock(&graph->num_visits_mutexes[neighbor]);
            graph->num_visits[v]++;
            //pthread_mutex_unlock(&graph->num_visits_mutexes[neighbor]);

            TaskData td = {parallel_dfs_visit, args};
            insert(pool.q, td);
            
            /* Start the tasks */
            runThreadPool(&pool);
        }
    }
}
```

