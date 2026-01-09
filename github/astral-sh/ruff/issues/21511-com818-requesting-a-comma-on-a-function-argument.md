---
number: 21511
title: COM818 requesting a comma on a function argument in multi-line formatting.
type: issue
state: closed
author: a8
labels:
  - question
assignees: []
created_at: 2025-11-18T15:12:26Z
updated_at: 2025-11-19T08:19:24Z
url: https://github.com/astral-sh/ruff/issues/21511
synced_at: 2026-01-07T13:12:16-06:00
---

# COM818 requesting a comma on a function argument in multi-line formatting.

---

_Issue opened by @a8 on 2025-11-18 15:12_

### Summary

Hi,

probably I'm doing something wrong.

Adding the COM lint rule in e. g. a pyproject.toml like

```
[project]
name = "blub"

[tool.ruff]
lint.select = [
    "COM",
]
```

for the code:

```
def f(arg):
    print(arg)

f(
    "argument"
)
```

a `ruff check test.py` complains:

```
 > ruff check code.py
COM812 [*] Trailing comma missing
 --> code.py:8:15
  |
7 | f(
8 |     "argument"
  |               ^
9 | )
  |
help: Add trailing comma

Found 1 error.
[*] 1 fixable with the `--fix` option.
```

And indeed, if I let it fix my function call it converts the argument of type string into a tuple.

```
> ruff check --fix code.py
Found 1 error (1 fixed, 0 remaining).
```

```
∝ ❯ cat code.py
#!/usr/bin/python3
"""Always add a docstring."""

def f(arg):
    print(arg)

f(
    "argument",
)
```

Note, the func call rewritten as:

```
f(
    "argument")
```

works like a charm.

Thanks,

Frank

### Version

ruff 0.14.3 and 0.1.5

---

_Label `question` added by @amyreese on 2025-11-18 16:07_

---

_Comment by @amyreese on 2025-11-18 17:02_

Adding that comma doesn't change the argument type to a tuple.

```
$ cat foo.py
def f(arg):
    print(arg, type(arg))

f(
    "argument"
)
f(
    "argument",
)
```
```
$ python foo.py
argument <class 'str'>
argument <class 'str'>
```

---

_Comment by @a8 on 2025-11-19 08:19_

Indeed. Sorry!

---

_Closed by @a8 on 2025-11-19 08:19_

---
