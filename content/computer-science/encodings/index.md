---
title: "Encodings"
date: 2023-11-23T18:22:21+04:00
draft: true
---

## Terminology

- **Character encoding** is the process of assigning numbers to graphical characters,
  especially the written characters of human language, allowing them to be stored,
  transmitted, and transformed using digital computers.

- **Characters** are the smallest element of writing that has a meaning.

- **Character sets** are a collection of characters that are useful, usually for a particular script or scripts.

- **Code Pages**(**Coded Character Sets**) are character sets where each character has been assigned a numerical representation.

- **Encodings** are a way to express a character set as actual coded data
  (how the characters get from character form to encoded byte form).

- A **code point** is a value or position of a character in a coded character set.

- A **code space** is the range of numerical values spanned by a coded character set.

- A **code unit** is the minimum bit combination that can represent a character in a character encoding.

- A **Character Repertoire** is the collection of characters included in a character set.

## A little bit of history

The first encodings are used for transfer information with a telegraph.

For example, Morse code, Baudot code(baud as a unit of measurement taken his name from this).

When computers became more smart, new encodings was appeared: EBCDIC, ASCII and UTF-8.

The ASCII allow you to represent numerals, Latin letters and other symbols.
Also it need only 7 bits to store character because of that you can use the codes 128-255 to add chars above the ASCII set.
The ANSI standard has defined code pages because of plenty of local additions to ASCII has existed.

The Unicode was a new way to think about chars.
A letter in different fonts has same code point(uppercase and lowercase letters have different code points).

This means that: A = _A_, **A** but A != a.

To understand text below take in my mind that memory was very expensive(10 Mb hard drive costed 250$) forty years ago.
For computer users who used only set characters of the Latin alphabet other sets were excessive
(to encode code points higher than the length of the code unit),
because of that the Unicode has used symbols **variable-length encodings** where an escape sequence would
signal that subsequent bits should be parsed as a higher code point.
The espape sequence is the **byte order mark** which is actually the `U+FEFF` char.

## Some facts

A lot of encoding replace unknown code points with `�` or `?`.

> **It does not make sense to have a string without knowing what encoding it uses**

In html this tag placed in head can help you set correct encoding:

```html
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
```

Also your web server can send this string in header of some response which will be sent before actual html:

```html
Content-Type: text/plain; charset="UTF-8"
```
