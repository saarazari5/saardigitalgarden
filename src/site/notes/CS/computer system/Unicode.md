---
{"dateCreated":"2022-11-22 19:09","tags":["computer_system","unicode","encoding"],"pageDirection":"ltr","dg-publish":true,"permalink":"/cs/computer-system/unicode/","dgPassFrontmatter":true}
---


# Unicode

Unicode also known as [Universal Coded Character Set](https://en.wikipedia.org/wiki/Universal_Coded_Character_Set) is really just another type of character encoding, it’s still a lookup of bits -> characters. The main difference between Unicode and ASCII is that Unicode allows characters to be up to 32 bits wide. That’s over 4 billion unique values. But for various reasons not all of that space will ever be used, there will actually only ever be 1,111,998 characters in Unicode.

But with Unicode, won’t all my documents, emails and web pages take up 4x the amount of space compared with ASCII? Well, luckily no. even though Unicode is a [[CS/computer system/wide-character\|wide-character]] set, together with Unicode comes several mechanisms to represent or encode the characters. These are primarily the UTF-8 and [[UTF-16\|UTF-16]] encoding schemes which both take a really smart approach to the size problem.

Unicode encoding schemes like [[CS/computer system/UTF-8\|UTF-8]] are more efficient in how they use their bits. With UTF-8, if a character can be represented with 1 byte that’s all it will use.

## Unicode Code Points
-   _Character_ is an overloaded term that can mean many things.
    
-   A _code point_ is the atomic unit of information. _Text_ is a sequence of code points. Each code point is a number which is given meaning by the Unicode standard.
    
-   A _code unit_ is the unit of storage of a _part_ of an encoded code point. In UTF-8 this means 8 bits, in UTF-16 this means 16 bits. A single code unit may represent a full code point, or part of a code point. For example, the snowman glyph (`☃`) is a single code point but 3 UTF-8 code units, and 1 UTF-16 code unit.
    
-   A _grapheme_ is a sequence of one or more code points that are displayed as a single, graphical unit that a reader recognizes as a single element of the writing system. For example, both `a` and `ä` are graphemes, but they may consist of multiple code points (e.g. `ä` may be two code points, one for the base character `a` followed by one for the diaeresis; but there's also an alternative, legacy, single code point representing this grapheme). Some code points are never part of any grapheme (e.g. the zero-width non-joiner, or directional overrides).
    
-   A _glyph_ is an image, usually stored in a _font_ (which is a collection of glyphs), used to represent graphemes or parts thereof. Fonts may compose multiple glyphs into a single representation, for example, if the above `ä` is a single code point, a font may choose to render that as two separate, spatially overlaid glyphs. For OTF, the font's GSUB and GPOS tables contain substitution and positioning information to make this work. A font may contain multiple alternative glyphs for the same grapheme, too.

