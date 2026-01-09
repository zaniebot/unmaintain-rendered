---
number: 16520
title: "Flag starred expressions in `return` and `yield`"
type: issue
state: closed
author: ntBre
labels:
  - bug
  - parser
assignees: []
created_at: 2025-03-05T16:31:15Z
updated_at: 2025-04-02T12:38:27Z
url: https://github.com/astral-sh/ruff/issues/16520
synced_at: 2026-01-07T13:12:16-06:00
---

# Flag starred expressions in `return` and `yield`

---

_Issue opened by @ntBre on 2025-03-05 16:31_

Both of these are invalid on every Python version I've tried and should be flagged as `ParseError`s in the parser:

```python
def f(): yield *rest
def f(): return *rest
```

They result in `SyntaxError: can't use starred expression here` on 3.8 and 3.13 and the less helpful `SyntaxError: invalid syntax` on 3.7.

This is separate from the related case in #16485 where these are part of a tuple, which is allowed after 3.8.

_Originally posted by @ntBre in https://github.com/astral-sh/ruff/issues/16485#issuecomment-2701182623_
            

---

_Label `parser` added by @ntBre on 2025-03-05 16:31_

---

_Label `bug` added by @MichaReiser on 2025-03-05 16:53_

---

_Comment by @AlexWaygood on 2025-03-05 19:22_

Python's `ast` module parses these:

```pycon
Python 3.11.4 (main, Sep 30 2023, 10:54:38) [GCC 11.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import ast
>>> ast.parse('def f(): return *x')
<ast.Module object at 0x7d5c2433d0>
```

which indicates to me that this syntax error is detected by CPython's compiler rather than CPython's parser. That means that this issue is a sub-issue of https://github.com/astral-sh/ruff/issues/11934. 

It doesn't mean that we shouldn't detect the syntax error (we absolutely should!). But it does mean that we may want a way of suppressing the diagnostic internally so that we can continue to test our parser directly against CPython's parser using e.g. the `py-fuzzer` script (which uses Python's `ast` module to generate random code).

---

_Comment by @MichaReiser on 2025-03-05 20:17_

Thanks @AlexWaygood That would suggest that this should be implemented as a semantic syntax error, outside the parser.

---

_Comment by @ntBre on 2025-03-05 21:21_

I'll start moving some of the other errors from #6591 to #11934 too. I think the `global` declarations in `try` nodes are another example:

```pycon
Python 3.12.9 (main, Feb 12 2025, 14:50:50) [Clang 19.1.6 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import ast
>>> ast.parse("""
... a=5
...
... def f():
...     try:
...         pass
...     except:
...         global a
...     else:
...         print(a)
... """)
<ast.Module object at 0x79c0c0e06d10>


---

_Comment by @AlexWaygood on 2025-03-05 21:23_

Good call. Some of the existing errors listed in https://github.com/astral-sh/ruff/issues/11934 and its sub-issues may also have been introduced in recent versions of CPython; we'll need to check when implementing those semantic syntax errors whether they should be emitted for all target versions or not

---

_Referenced in [astral-sh/ruff#16485](../../astral-sh/ruff/pulls/16485.md) on 2025-03-06 09:40_

---

_Comment by @dhruvmanila on 2025-03-06 09:40_

(I think from Python 3.9, you can test the CPython parser directly using `python -m ast path/to/file.py`.)

---

_Comment by @ntBre on 2025-03-07 19:27_

This issue may also include `for` statements as added in 3.9 ([BPO](https://github.com/python/cpython/issues/90881)).

``` pycon
Python 3.13.1 (main, Dec  4 2024, 18:05:56) [GCC 14.2.1 20240910] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import ast
>>> ast.parse("for _ in *rest:...")
<ast.Module object at 0x7cd78d9bb450>
>>> for _ in *rest: ...
  File "<python-input-2>", line 1
    for _ in *rest: ...
             ^^^^^
SyntaxError: can't use starred expression here
>>>
```

---

_Referenced in [astral-sh/ruff#16558](../../astral-sh/ruff/pulls/16558.md) on 2025-03-07 19:42_

---

_Referenced in [astral-sh/ruff#11934](../../astral-sh/ruff/issues/11934.md) on 2025-04-01 19:10_

---

_Referenced in [astral-sh/ruff#17134](../../astral-sh/ruff/pulls/17134.md) on 2025-04-01 20:21_

---

_Closed by @ntBre on 2025-04-02 12:38_

---

_Closed by @ntBre on 2025-04-02 12:38_

---
