---
title: "Fix encoding after unzip"
date: 2023-11-23T18:22:21+04:00
draft: true
---

## UnZip Locale Issues

These issues are thought to be fixed in the patch. But since none of the editors have data to test this, the following workarounds are retained in case they might still be needed.

The UnZip package assumes that filenames stored in the ZIP archives created on non-Unix systems are encoded in CP850, and that they should be converted to ISO-8859-1 when writing files onto the filesystem. Such assumptions are not always valid. In fact, inside the ZIP archive, filenames are encoded in the DOS codepage that is in use in the relevant country, and the filenames on disk should be in the locale encoding. In MS Windows, the OemToChar() C function (from User32.DLL) does the correct conversion (which is indeed the conversion from CP850 to a superset of ISO-8859-1 if MS Windows is set up to use the US English language), but there is no equivalent in Linux.

When using unzip to unpack a ZIP archive containing non-ASCII filenames, the filenames are damaged because unzip uses improper conversion when any of its encoding assumptions are incorrect. For example, in the ru_RU.KOI8-R locale, conversion of filenames from CP866 to KOI8-R is required, but conversion from CP850 to ISO-8859-1 is done, which produces filenames consisting of undecipherable characters instead of words (the closest equivalent understandable example for English-only users is rot13).

### To fix encoding after unzip archive zipped on windows you need to use:

```bash
find -mindepth 1 -exec sh -c 'mv "$1" "$(echo "$1" | iconv -f ISO_8859-1 -t cp850 | iconv -f cp866 )"' sh {} \;
```
