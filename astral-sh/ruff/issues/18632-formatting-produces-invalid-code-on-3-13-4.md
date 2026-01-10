```yaml
number: 18632
title: Formatting produces invalid code on 3.13.4
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - formatter
  - parser
  - python313
  - python314
assignees: []
created_at: 2025-06-11T19:33:19Z
updated_at: 2025-06-17T05:39:44Z
url: https://github.com/astral-sh/ruff/issues/18632
synced_at: 2026-01-10T11:09:58Z
```

# Formatting produces invalid code on 3.13.4

---

_Issue opened by @MeGaGiGaGon on 2025-06-11 19:33_

### Summary

With the release of 3.13.4, https://github.com/python/cpython/issues/129958 was fixed, which now makes formatting produce invalid code on 3.13.4
[playground link](https://play.ruff.rs/442b84a5-9df9-40fd-90ec-aa29d29cdef0)
Input:
```py
f"{
11111111111111111111111111111111111111111111111111111111111111111111111111111111111111111:}"
```
Output:
```py
f"{
    11111111111111111111111111111111111111111111111111111111111111111111111111111111111111111:
}"
```
The input is valid code on 3.13.4, the output is not
```
$ uv run --python 3.13.4 python -c $'f"{\n11111111111111111111111111111111111111111111111111111111111111111111111111111111111111111:}"'

$ uv run --python 3.13.4 python -c $'f"{\n    11111111111111111111111111111111111111111111111111111111111111111111111111111111111111111:\n}"'
  File "<string>", line 2
    1111111111111111111111111111111111111111111111111111111111111111111111111111
1111111111111:

              ^
SyntaxError: f-string: newlines are not allowed in format specifiers for single
quoted f-strings
```

### Version

_No response_

---

_Label `formatter` added by @ntBre on 2025-06-11 20:24_

---

_Label `parser` added by @ntBre on 2025-06-11 20:28_

---

_Comment by @ntBre on 2025-06-11 20:30_

It looks like the PR linked in that issue modifed the CPython [lexer](https://github.com/python/cpython/pull/130063/files#diff-4903ddce642a378ef03f37f72332cf1f5acb5bb0c6247669145fc3d8ea7cd43e), so we may need to update that as well.

---

_Comment by @ntBre on 2025-06-11 20:30_

Thanks for the report!

---

_Label `python314` added by @AlexWaygood on 2025-06-11 20:41_

---

_Comment by @MeGaGiGaGon on 2025-06-11 20:43_

(@AlexWaygood I think the label should be `python313` instead of `python314`, since the affected version is 3.13.4 (Yes it's confusing, I also typed `3.14` on accident a bunch while writing the issue))

---

_Comment by @AlexWaygood on 2025-06-11 20:45_

Oh wow, sorry, I misread!

---

_Label `python313` added by @AlexWaygood on 2025-06-11 20:45_

---

_Comment by @MichaReiser on 2025-06-12 05:42_

Hmm, it's unfortunate that this changes in a patch release. 

But yes. We'll have to update the formatter. We may want to wait with updating the parser until the formatter is rolled out so that the formatter can fix incorrectly formatted code.

---

_Comment by @MichaReiser on 2025-06-12 05:54_

I'm still trying to understand the extend of the bug here. Reading through https://github.com/python/cpython/issues/110259 it seems that format strings followed by a newline are fine. 

But it seems that https://github.com/python/cpython/issues/129958 now goes further and disallows any format spec that's followed by a newline, even if, what comes after, is only whitespace. I think this is very unfortunate for formatters. I can understand the motivation in https://github.com/python/cpython/issues/110259 but the motivation for disallowing any newline followed only by whitespace isn't clear to me. It seems an unnecessary restriction IMO.

Either way. It seems we can no longer split f-string parts with format specifiers 🤷 

---

_Comment by @MeGaGiGaGon on 2025-06-12 15:06_

At least the change itself makes sense to me, since as I understand it the allowing newlines was causing the lexer to reset to normal mode in the format spec, which causes issues down the line. It also behaves very strangely, ie using
```py
class A:
    def __format__(self, *args):
        print(args)
        return str(self)
```

Under 3.12 this
```py
f"{A(): }"
```
outputs `(' ',)`
while this
```py
f"{A():
 }"
```
outputs `('',)`

The argument also makes a sort of sense to me, since the format spec is basically just a string without quotes that gets passed to `__format__`, so the logic being used is that the unquoted string acts like the string it's in, and since single quoted strings can't have newlines then their format specs can't either.

---

_Comment by @MichaReiser on 2025-06-13 15:38_

This might be easier to fix than expected. It seems we have already implemented the same behavior for triple quoted strings. We just need to apply it not to all strings

https://github.com/astral-sh/ruff/blob/c9dff5c7d5bcab2a83f1c3b4a1a80af230ece541/crates/ruff_python_formatter/src/other/interpolated_string_element.rs#L98-L119

---

_Comment by @MichaReiser on 2025-06-13 16:53_

And we probably need to revert https://github.com/astral-sh/ruff/pull/7787

What I understand so far is that it's fine for a format-spec to contain line breaks inside of interpolated elements. It's just not allowed inside of string elements

---

_Assigned to @MichaReiser by @MichaReiser on 2025-06-13 16:55_

---

_Comment by @MeGaGiGaGon on 2025-06-13 22:44_

After some messing around, I was able to come up with [this](https://play.ruff.rs/661480dc-1cd6-41a9-9014-87e92c503196)
```py
rf"{1:\
\}"
```
formats to
```py
rf"{
    1:\
\
}"
```
So even without the changes in 3.13.4 it was still technically possible to have behavior change as a result of the reformatting
```py
>>> rf"{1:\
... \}"
Traceback (most recent call last):
  File "<python-input-5>", line 1, in <module>
    rf"{1:\
       ^^^^
    \}"
    ^^
ValueError: Unknown format code '\' for object of type 'int'
>>> rf"{
...     1:\
... \
... }"
'1'
```

---

_Closed by @MichaReiser on 2025-06-17 05:39_

---
