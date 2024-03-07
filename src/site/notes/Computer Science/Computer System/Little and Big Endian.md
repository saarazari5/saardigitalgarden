---
{"dateCreated":"2022-09-29 20:23","tags":["computer_system","computer_science"],"pageDirection":"ltr","dg-publish":true,"permalink":"/computer-science/computer-system/little-and-big-endian/","dgPassFrontmatter":true}
---



# Little and Big Endian
Little and big endian are two ways of storing multibyte data-types ( int, float, etc). In little endian machines, last byte of binary representation of the multibyte data-type is stored first. On the other hand, in big endian machines, first byte of binary representation of the multibyte data-type is stored first.

Suppose integer is stored as 4 bytes (For those who are using [[DOS-based compilers\|DOS-based compilers]] such as C++ 3.0, integer is 2 bytes) then a variable x with value 0x01234567 will be stored as following:

![Pasted image 20220929202647.png](/img/user/Assets/Pasted%20image%2020220929202647.png)

**How to see memory representation of multibyte data types on your machine?**   
Here is a sample C code that shows the byte representation of int, float and pointer.

```c++

#include <bits/stdc++.h>
using namespace std;
int main()
{
	unsigned int i = 1;
	char *c = (char*)&i;
	if (*c)
		cout<<"Little endian";
	else
		cout<<"Big endian";
	return 0;
}
```

**Does endianness matter for programmers?**   
Most of the times compiler takes care of endianness, however, endianness becomes an issue in following cases.  
It matters in network programming: Suppose you write integers to file on a little endian machine and you transfer this file to a big-endian machine. Unless there is little endian to big endian transformation, big endian machine will read the file in reverse order. You can find such a practical example here.  
Standard byte order for networks is big endian, also known as network byte order. Before transferring data on network, data is first converted to network byte order (big endian).   
Sometimes it matters when you are using type casting, below program is an example.

