---
number: 16318
title: Newlines in multiline strings are not normalized to line feeds
type: issue
state: open
author: dscorbett
labels:
  - bug
assignees: []
created_at: 2025-02-22T18:21:02Z
updated_at: 2025-02-24T11:12:57Z
url: https://github.com/astral-sh/ruff/issues/16318
synced_at: 2026-01-07T13:12:16-06:00
---

# Newlines in multiline strings are not normalized to line feeds

---

_Issue opened by @dscorbett on 2025-02-22 18:21_

### Description

Ruff 0.9.7 does not normalize all newlines (`"\n"`, `"\r"`, `"\r\n"`) to line feeds (`"\n"`) within multiline strings, as Python does, which causes errors in rules that check the length or contents of string literals, including:

* [`strip-with-multi-characters` (B005)](https://docs.astral.sh/ruff/rules/strip-with-multi-characters/)
* [`duplicate-value` (B033)](https://docs.astral.sh/ruff/rules/duplicate-value/)
* [`multi-value-repeated-key-literal` (F601)](https://docs.astral.sh/ruff/rules/multi-value-repeated-key-literal/)
* [`hardcoded-string-charset` (FURB156)](https://docs.astral.sh/ruff/rules/hardcoded-string-charset/)
* [`non-unique-enums` (PIE796)](https://docs.astral.sh/ruff/rules/non-unique-enums/)
* [`repeated-keyword-argument` (PLE1132)](https://docs.astral.sh/ruff/rules/repeated-keyword-argument/)
* [`bad-str-strip-call` (PLE1310)](https://docs.astral.sh/ruff/rules/bad-str-strip-call/)
* [`duplicate-literal-member` (PYI062)](https://docs.astral.sh/ruff/rules/duplicate-literal-member/)
* [`split-static-string` (SIM905)](https://docs.astral.sh/ruff/rules/split-static-string/)

```console
$ printf 'from enum import Enum
from typing import Literal

# False negatives without fixes:
"\\r".strip("""\n\r""")  # B005, PLE1310
dict(**{"""\n""": 1, """\r""": 2})  # F601, PLE1132
class E(Enum):  # PIE796
    A = """\n"""
    B = """\r"""

# False negatives that should have fixes:
{"""\n""", """\r"""}  # B033
x: Literal["""\n""", """\r"""]  # PYI062

print("False positive with wrong fix:")
print(len(set(""" \t\n\r\v\f""")))  # FURB156

print("True positive with wrong fix:")
print(len("a\\nb\\nc".split("""\r""")))  # SIM905
' | tee newlines.py.bak >newlines.py

$ python newlines.py
False positive with wrong fix:
5
True positive with wrong fix:
3

$ ruff --isolated check --preview --select B005,B033,F601,FURB156,PIE796,PLE1132,PLE1310,PYI062,SIM905 newlines.py --fix
Found 2 errors (2 fixed, 0 remaining).

$ python newlines.py
False positive with wrong fix:
6
True positive with wrong fix:
1
```

Those rules should behave as follows, simulated by normalizing all the newlines to line feeds before running Ruff.
```console
$ tr '\r' '\n' <newlines.py.bak >line_feeds.py

$ ruff --isolated check --preview --select B005,B033,F601,FURB156,PIE796,PLE1132,PLE1310,PYI062,SIM905 line_feeds.py --fix --show-fixes --output-format grouped
line_feeds.py:
   5:1  B005 Using `.strip()` with multi-character strings is misleading
   5:12 PLE1310 String `strip` call contains duplicate characters
   9:9  F601 Dictionary key literal repeated
   9:9  PLE1132 Repeated keyword argument: `
`
  14:5  PIE796 Enum contains duplicate value: `"""\n"""`


Fixed 3 errors:
- line_feeds.py:
    1 × PYI062 (duplicate-literal-member)
    1 × B033 (duplicate-value)
    1 × SIM905 (split-static-string)

Found 8 errors (3 fixed, 5 remaining).

$ python line_feeds.py | grep -v wrong
5
3
```


---

_Label `bug` added by @ntBre on 2025-02-22 18:33_

---

_Comment by @yahayaohinoyi on 2025-02-23 03:53_

can i get assigned @ntBre 

---

_Comment by @ntBre on 2025-02-23 03:54_

Sure!

---

_Assigned to @yahayaohinoyi by @ntBre on 2025-02-23 03:54_

---

_Comment by @yahayaohinoyi on 2025-02-23 22:26_

I need some help understanding the problem statement here;

- "\\r".strip("""\n\r""")  -> ruff should complain about this since \n\r are separate characters (**this makes sense to me**. If we convert all \r \rn to \n, doens't that contradict in some way?)
- "dict(**{"""\n""": 1, """\r""": 2})" -> I might be missing something here but `why do we want ruff to flag this on F601`?
- same as; `class E(Enum):  # PIE796
    A = """\n"""
    B = """\r""" 
`

My main confusion here is if we normalize newline characters, then "\\r".strip("""\n\r""") shouldn't be a false negative and should be just fine?

Looking forward to your reply :) @dscorbett 


---

_Comment by @dscorbett on 2025-02-23 23:21_

Python normalizes all newlines to line feeds in the source code, but that only applies to literal newlines, not to escape sequences in strings that represent newlines. The problem is that Ruff does not understand that: if there is a carriage return (and I do mean an actual carriage return, not the escape sequence `\r`) inside a string, Ruff treats it as a carriage return, but Python treats it as a line feed.

For example, this:
```
"\\r".strip("""\n\r""")
```
is a snippet of a string in shell syntax which printf converts to this Python code:
```python
"\r".strip("""

""")
```
That Python code has two characters in the second string literal: a line feed followed by a carriage return. (The distinction between line feeds and carriage returns is lost on a web page, which is why I showed how to create the code using printf on the command line.)

Ruff treats that code like:
```python
"\r".strip("\n\r")
```

However, Python normalizes all newlines to line feeds, so the code is actually equivalent to:
```python
"\r".strip("\n\n")
```

So, in fact, the `strip` argument contains duplicate line feeds, so it is appropriate for [strip-with-multi-characters (B005)](https://docs.astral.sh/ruff/rules/strip-with-multi-characters/) and [bad-str-strip-call (PLE1310)](https://docs.astral.sh/ruff/rules/bad-str-strip-call/) to report it. Similarly, the F601 and PIE796 examples are false negatives because Ruff thinks the strings are not equal, but they are actually equal. All of my examples are like that: each rule checks whether two strings are equal, but whenever there is a literal carriage return in the Python file, the rule wrongly treats it like a carriage return.

The solution is probably to fix the tokenizer or something fundamental like that. The solution is not to fix each rule individually.

---

_Comment by @ntBre on 2025-02-24 00:05_

Thanks @dscorbett for the clarification, and sorry @yahayaohinoyi, I should have noticed how complicated this was earlier. It does sound to me like this will require a low-level change to our tokenizer and/or parser, and then the rules will hopefully just work properly.

@yahayaohinoyi I can leave you assigned to this if you're interested in looking into it at that level, but don't feel any pressure to. This might be the kind of thing we need to look into internally.

---

_Comment by @dhruvmanila on 2025-02-24 08:31_

I don't think updating the tokenizer is the right solution here. I'll need to spend some time here (although not a high priority right now) as to what needs to be done but it does seem that the CPython parser _is_ normalizing the newlines to line feeds so it might require updating the parser:

**Source:**
```console
$ bat -A parser/_.py
         File: parser/_.py
     1 ~ "\r".strip("""␍␊
     2 ~ ␊
     3 ~ ␍␍""")
```

**CPython tokenizer and parser output:**
```console
$ python -m tokenize parser/_.py                  
0,0-0,0:            ENCODING       'utf-8'        
1,0-1,4:            STRING         '"\\r"'        
1,4-1,5:            OP             '.'            
1,5-1,10:           NAME           'strip'        
1,10-1,11:          OP             '('            
1,11-3,5:           STRING         '"""\r\n\n\r\r"""'
3,5-3,6:            OP             ')'            
3,6-3,7:            NEWLINE        ''             
4,0-4,0:            ENDMARKER      ''          

$ python -m ast parser/_.py     
Module(
   body=[
      Expr(
         value=Call(
            func=Attribute(
               value=Constant(value='\r'),
               attr='strip',
               ctx=Load()),
            args=[
               Constant(value='\n\n\n\n')],
            keywords=[]))],
   type_ignores=[])
```

---

_Referenced in [astral-sh/ruff#16342](../../astral-sh/ruff/issues/16342.md) on 2025-02-24 08:46_

---

_Comment by @yahayaohinoyi on 2025-02-24 11:12_

> Thanks @dscorbett for the clarification, and sorry @yahayaohinoyi, I should have noticed how complicated this was earlier. It does sound to me like this will require a low-level change to our tokenizer and/or parser, and then the rules will hopefully just work properly.
> 
> @yahayaohinoyi I can leave you assigned to this if you're interested in looking into it at that level, but don't feel any pressure to. This might be the kind of thing we need to look into internally.

Thanks. No pressure. I'd update with anything useful I find 

---
