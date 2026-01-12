```yaml
number: 131
title: "ignore patterns with non-ASCII Unicode literals aren't allowed"
type: issue
state: closed
author: elmart
labels:
  - bug
assignees: []
created_at: 2016-09-28T23:12:00Z
updated_at: 2016-10-10T23:29:12Z
url: https://github.com/BurntSushi/ripgrep/issues/131
synced_at: 2026-01-12T18:23:11Z
```

# ignore patterns with non-ASCII Unicode literals aren't allowed

---

_@elmart_

When I do `rg <pattern-with-diacritics>`, I get:

``` 
Error parsing regex near 'TopÑAPA\.' at character offset 6769: Unicode features are not allowed when the Unicode (u) flag is not set
```

I also get that error when using this in combination with fzf, which is strange because there, rg output is only filenames, which has no non-ascii chars, as far as I know.

I'm on OSX 10.11.6, and my locale is en_US.UTF-8


---

_Comment by @BurntSushi on 2016-09-28 23:15_

Could you include the full command you are running?


---

_Comment by @elmart on 2016-09-28 23:28_

I'm running just `rg café`. 
The literal 'TopÑAPA', mentioned in the error string, is indeed within some of my files. 


---

_Comment by @BurntSushi on 2016-09-28 23:33_

I'm just having a hard time understanding, because that error message to me implies that `TopÑAPA` was in your pattern...

Is there anyway you can provide a full reproduction? I.e., the full command and a file that you are searching that causes the problem?


---

_Comment by @BurntSushi on 2016-09-28 23:34_

The error message also implies that the pattern is over 6,000 characters.


---

_Comment by @elmart on 2016-09-28 23:39_

Yes, I also had difficulties to try to understand the error message. 
I'll try to narrow this down and provide you something reproducible. 
But can't do it now. I'll come back soon. 
Thx. 


---

_Comment by @BurntSushi on 2016-09-28 23:48_

Thanks! To be clear, I can run `rg café` without any issues and see correct results.

To provide more context, if I run `rg '(?-u)café'`, where that `(?-u)` disables Unicode support, then I get your error message:

```
Error parsing regex near ')café' at character offset 9: Unicode features are not allowed when the Unicode (u) flag is not set.
```

In this case, it's expected behavior, because when Unicode mode is disabled, you lose the ability to search for Unicode string literals. (I can go into more gruesome detail here if anyone is curious, but it's actually a design decision in the regex engine, not `ripgrep` itself.)

Anyway, I kind of expect that there's some weirdness going on, so I shall look forward to more details. :-) Thank you for looking into it!


---

_Label `question` added by @BurntSushi on 2016-09-28 23:49_

---

_Comment by @samlh on 2016-09-29 23:27_

It is something with .gitignore parsing - I can repro by running `c:\Users\samh\bin\rg.exe --files-with-matches "(?-u)." "c:\foo"` on a folder with only the following .gitignore file:

```
Offset(h) 00 01 02 03 04 05 06 07 08 09 0A 0B 0C 0D 0E 0F

00000000  EF BB BF 6E 6F 64 65 5F 6D 6F 64 75 6C 65 73 0D  ï»¿node_modules.
00000010  0A 61 7A 75 72 65 5F 65 72 72 6F 72              .azure_error
```

yielding:

```
Error parsing regex near '\\])﻿node_' at character offset 28: Unicode features are not allowed when the Unicode (u) flag is not set.
```


---

_Comment by @BurntSushi on 2016-09-29 23:40_

Ah, I got it now. The globber translates patterns to regexes and disables Unicode support. But this will fail if any literal is not an ASCII codepoint.


---

_Label `bug` added by @BurntSushi on 2016-09-29 23:40_

---

_Label `question` removed by @BurntSushi on 2016-09-29 23:40_

---

_Comment by @elmart on 2016-09-30 10:50_

Yes! I can confirm that.
'TopÑapa' literal is present in one of my .gitignore files.
Removing that makes it all work again.


---

_Comment by @afk-mario on 2016-10-03 18:58_

I get a similar error, I guess is my .gitignore file, but don't know how to search for '\])# Cr' 

```
rg --debug hola                                                                                                                  Unity/kleptocat develop
DEBUG:grep::search: regex ast:
Literal {
    chars: [
        'h',
        'o',
        'l',
        'a'
    ],
    casei: false
}
DEBUG:grep::literals: literal prefixes detected: Literals { lits: [Complete(hola)], limit_size: 250, limit_class: 10 }
Error parsing regex near '\\])\# Cr' at character offset 28: Unicode features are not allowed when the Unicode (u) flag is not set.
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
```


---

_Renamed from "Unicode issues" to "ignore patterns with non-ASCII Unicode literals aren't allowed" by @BurntSushi on 2016-10-10 21:29_

---

_Comment by @BurntSushi on 2016-10-10 23:29_

fixed in `e96e930`


---

_Closed by @BurntSushi on 2016-10-10 23:29_

---
