---
{"dateCreated":"2022-11-22 19:47","tags":["computer_system","unicode","encoding"],"pageDirection":"ltr","dg-publish":true,"permalink":"/computer-science/computer-system/wide-character/","dgPassFrontmatter":true}
---


# wide-character

A wide character is a 2-byte multilingual character code. Any character in use in modern computing worldwide, including technical symbols and special publishing characters, can be represented according to the Unicode specification as a wide character.  A wide character is a computer character datatype that generally has a size greater than the traditional 8-bit character. The increased datatype size allows for the use of larger coded character sets. That means that every encoding standard that uses his characters can grow to a size larger then 8 bits should use wide chars to hold the data of the char (for example [[Computer Science/Computer System/UTF-8\|UTF-8]] or [[UTF-16\|UTF-16]])

## relation to UCS and [[Computer Science/Computer System/Unicode\|Unicode]]
A wide character refers to the size of the datatype in memory. It does not state how each value in a character set is defined. Those values are instead defined using character sets

## wide chars and endianness
There is no guarantee whenever wide char will be effected from endianness or not, because it is just a data type and not an actual encoding implementation. Each operating system use its own encoding standard, For example, UTF-16 little endian is the standard for Windows. 
The width of wchar_t is compiler-specific and can be as small as 8 bits. Consequently, programs that need to be portable across any C or C++ compiler should not use wchar_t for storing Unicode text. The wchar_t type is intended for storing compiler-defined wide characters, which may be Unicode characters in some compilers.
