```yaml
number: 6078
title: "SyntaxError when unpacking sequences inside of []"
type: issue
state: closed
author: Morkunas
labels:
  - bug
  - parser
assignees: []
created_at: 2023-07-25T21:06:13Z
updated_at: 2023-07-28T04:28:03Z
url: https://github.com/astral-sh/ruff/issues/6078
synced_at: 2026-01-12T15:54:45Z
```

# SyntaxError when unpacking sequences inside of []

---

_@Morkunas_

Ruff version: 0.0.280
Python version: 3.8.10

Command run:
`ruff check code.py`
Contents of `code.py`:

```
c = {}
c[(*[1, 2]), 3] = 4
```

This is a valid Python 3.8 code, which creates a dictionary `{(1, 2, 3): 4}`. The above invocation of ruff however throws this error:

`E999 SyntaxError: cannot use starred expression here`


---

_Comment by @charliermarsh on 2023-07-25 21:13_

Thanks!

---

_Label `bug` added by @charliermarsh on 2023-07-25 21:13_

---

_Label `parser` added by @konstin on 2023-07-26 10:33_

---

_Comment by @MichaReiser on 2023-07-27 14:43_

Fixing this requires that our parser supports parsing different python versions by passing a *python version* to the parser and respecting it. 

---

_Comment by @charliermarsh on 2023-07-27 15:02_

Why is that required? Don't we just parse a superset?

---

_Comment by @MichaReiser on 2023-07-27 15:58_

> Why is that required? Don't we just parse a superset?

We could do that, but I don't think that's what the parser does today. I would expect my linter to warn me about this syntax error when I target Python 3.12, so that I don't have to find out at runtime.

---

_Comment by @charliermarsh on 2023-07-27 16:10_

But that’s valid syntax under Python 3.12. Do you mean, if you target Python 3.7?

---

_Comment by @charliermarsh on 2023-07-27 16:11_

And I believe that _is_ what the parser does today. We parse all valid Python 3.12 syntax, even if the user is targeting earlier Python versions. But I may be misunderstanding your comment.

---

_Comment by @MichaReiser on 2023-07-27 16:16_

> But that’s valid syntax under Python 3.12. Do you mean, if you target Python 3.7?

I need to double check but I pasted it into Python 3.12 and it failed with a syntax error

---

_Comment by @charliermarsh on 2023-07-27 16:52_

Huh, interesting... I agree, it seems to work on Python 3.8 but not later versions. That's very confusing to me, I had thought that they'd increased the number of locations in which starred expressions were supported, not that they'd made backwards incompatible changes. We can't really support this right now though, since we don't support version upper-bounds, only lower-bounds.

---

_Comment by @charliermarsh on 2023-07-27 16:58_

Related: https://github.com/astral-sh/ruff/issues/2321

---

_Comment by @zanieb on 2023-07-27 17:02_

Yeah interesting...

```
Python 3.7.17
OK

Python 3.8.17
OK

Python 3.9.17
  File "<string>", line 1
    c = {}; c[(*[1, 2]), 3] = 4
               ^
SyntaxError: can't use starred expression here

Python 3.10.12
  File "<string>", line 1
    c = {}; c[(*[1, 2]), 3] = 4
               ^^^^^^^
SyntaxError: cannot use starred expression here

Python 3.11.4
  File "<string>", line 1
    c = {}; c[(*[1, 2]), 3] = 4
               ^^^^^^^
SyntaxError: cannot use starred expression here

Python 3.12.0b4+
  File "<string>", line 1
    c = {}; c[(*[1, 2]), 3] = 4
               ^^^^^^^
SyntaxError: cannot use starred expression here
```

---

_Comment by @charliermarsh on 2023-07-27 17:03_

My best guess is that this was an unintentional change from [PEP 646](https://peps.python.org/pep-0646/#grammar-changes).

---

_Comment by @zanieb on 2023-07-27 17:04_

Agreed — I haven't found a bug report upstream yet. I could open one unless someone else knows of an existing issue.

---

_Comment by @charliermarsh on 2023-07-28 04:28_

I read through this issue in CPython which was flagged to me on [Twitter](https://twitter.com/isidentical/status/1684683536790335489): https://github.com/python/cpython/issues/84811#issuecomment-1093871867.

It sounds like Guido effectively considers it a bug that this worked in earlier versions of Python 3. I am inclined to mark this as `wontfix`, since it stopped working some time around Python 3.9 ([Twitter](https://twitter.com/mgdotdev/status/1684615055692951552)) and wasn't an intentional part of the grammar.

---

_Closed by @charliermarsh on 2023-07-28 04:28_

---
