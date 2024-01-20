---
{"dateCreated":"2024-01-18 22:17","tags":["networks"],"pageDirection":"rtl","dg-publish":true,"permalink":"/computer-science/networks/socket-programming/","dgPassFrontmatter":true}
---

# תכנות ב Sockets 
צורת התקשורת ב[[Computer Science/Programming Concepts/Server side\|שרת-לקוח]] היא צורת התקשורת הנפוצה ביותר באינטרנט. היא נמצאת כמעט בכל תוכנה במחשב שלנו. 
כדי שצורת תקשורת כזאת תעבוד יש צורת בממשק שמאפשר לחבר בין end systems ללא תלות בחומרה שלהם. הממשק הזה נקרא __Socket__.

socket הוא נקודת קצה של חיבור בין שני רכיבים. הצינור הוא [[Computer Science/Networks/Computer Networks Intro and Protocol layers#ארכיטקטורת שכבות\|מודל השכבות]] ונקודות הקצה הן הsockets. 

כדי להגדיר socket יש צורך במספר דברים:
א) אתחול של הsocket
ב) כתובת [[Computer Science/Networks/IP\|IP]] ו Port שמשותפות בין השרת ללקוח.
ג) באפר שקובע כמה בתים הסוקט יכול לקבל/להעביר בבקשה בודדת.

## TCP Socket
נסתכל על דוגמה פשוטה של echo server בפייתון שמתמש ב TCP Socket ונסביר כל שורה

```Python
import socket

# Create a socket object
server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Bind the socket to a specific address and port
host = '127.0.0.1'  # Loopback address (localhost)
port = 12345        # Choose any available port
server_socket.bind((host, port))

# Listen for incoming connections
server_socket.listen(5)  # Maximum 5 connections in the queue

print(f"Server listening on {host}:{port}")

while True:
    # Wait for a client to connect
    client_socket, client_address = server_socket.accept()

    print(f"Accepted connection from {client_address}")

    # Receive and echo back data
    while True:
        data = client_socket.recv(1024)  # Receive data in chunks of 1024 bytes
        if not data:
            break  # No more data, end the connection

        # Echo the received data back to the client
        client_socket.sendall(data)

    print(f"Connection with {client_address} closed")

    # Close the client socket
    client_socket.close()

# Close the server socket (this line will not be reached in this example)
server_socket.close()

```

א) __יצירת הsocket__
`server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)`
שורה זאת מייצרת socket חדש כאשר `AF_INET` מתייחס למשפחת כתובות ה IP מגרסה IPv4.
הפרמטר `SOCK_STREAM` מתייחס לכך שסוג הסוקט הוא [[Computer Science/Networks/Transport Layer#TCP\|TCP]].

ב) __חיבור הsocket לפורט ו IP__
```Python
host = '127.0.0.1'
port = 12345
server_socket.bind((host, port))
```

ג) __האזנה לחיבורים__
`server_socket.listen(5)` - מאפשר לsocket לקבל 5 חיבורים בתור. אם יהיו 5 חיבורים מלקוחות עליו יהיה לחכות שאחד הלקוחות יסיים כדי לקבל חדשים.

ד)__קבלת חיבורים חדשים בלולאה__
```Python
while True:
    client_socket, client_address = server_socket.accept()
```
כאשר `accept` זאת פונקציה שחוסמת קריאות ומחכה עד שלקוח מתחבר לשרת. ברגע שלקוח מתחבר הפונקציה מחזירה את האובייקט socket עצמו ואת הכתובת שלו. 

ה) __קבלת מידע מהלקוח והחזרתו ללקוח בצורת echo__
```Python
while True:
    data = client_socket.recv(1024)
    if not data:
        break
    client_socket.sendall(data)
```
זאת פונקציה שחוסמת עד שהלקוח שולח מידע בגודל קטן או שווה ל$1024$ bytes. כלומר $1024$ הוא גודל הבאפר שהגדרנו מקודם.

ו) __סגירת הsockets__
`client_socket.close(); server_socket.close` .
ברור מה הפונקציות הללו עושות.


>[!info] זיהוי TCP Socket
>המזהה של TCP Socket הוא הרביעייה $(\text{Src IP, Src Port, Dst IP, Dst Port})$ המשמעות של זה היא שגם אם נעשה bind לאותו IP ו Port משני Sockets שונים בצד השרת, ייפתחו שני סוקטים שונים לטובת קבלת המידע.
## UDP Socket 
נבנה echo server מבוסס [[Computer Science/Networks/Transport Layer#UDP\|UDP]] ונסביר את ההבדלים

```Python
import socket

server_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
host = '127.0.0.1'
port = 12345
server_socket.bind((host, port))

print(f"Server listening on {host}:{port}")

while True:
    data, client_address = server_socket.recvfrom(1024)
    print(f"Received data from {client_address}: {data.decode()}")
    server_socket.sendto(data, client_address)
```

ההבדל הראשון הוא בסוג הsocket, בTCP זה היה `SOCK_STREAM` בעוד שUDP משתמש ב `SOCK_DGRAM`. ההבדל בינהם הוא משמעותי אך לא אתייחס לזה כאן. 

כמו כן, נשים לב ש TCP צריך לבצע חיבור מפורש ואמין באמצעות`accept`, בניגוד ל UDP שלא מצריך חיבור לפני שליחה וקבלת מידע. כלומר אנחנו מקבלים מידע מכל לקוח שישלח לפורט ולIP שלנו מידע בלי צורך בעבודה מול הsocket של הלקוח.

נשים לב שמימוש ב UDP הוא פשוט בהרבה והסיבה היא שהוא פרוטוקול ״לא אמין״ של שכבת התעבורה.


>[!info] זיהוי UDP Socket
>המזהה של UDP Socket הוא סך הכל $(\text{Dst IP, Dst Port})$  מה שמאפשר לצד שרת לקבל באותו socket מידע ממספר רב של לקוחות.

