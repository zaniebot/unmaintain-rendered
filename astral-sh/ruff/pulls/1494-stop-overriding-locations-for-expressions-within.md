```yaml
number: 1494
title: Stop overriding locations for expressions within f-strings
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: remove-in_f_string
created_at: 2022-12-31T04:10:55Z
updated_at: 2022-12-31T04:49:06Z
url: https://github.com/astral-sh/ruff/pull/1494
synced_at: 2026-01-12T15:55:06Z
```

# Stop overriding locations for expressions within f-strings

---

_@harupy_

https://github.com/RustPython/RustPython/pull/4384 fixed the location of `FormattedValue`. Now, expressions within f-strings should have the correct locations. Here's a quick demo.


https://user-images.githubusercontent.com/17039389/210124329-b0c71bd5-3c9a-4b44-949a-9f774f56fac9.mov

```
> cargo run -- --show-source --no-cache --select C,F a.py
a.py:1:4: C408 Unnecessary `list` call (rewrite as a literal)
  |
1 | f"{list()}"
  |    ^^^^^^ C408
  |
a.py:2:6: F821 Undefined name `x`
  |
2 | f"{  x   }"
  |      ^ F821
  |
a.py:4:6: C408 Unnecessary `dict` call (rewrite as a literal)
  |
4 | f"""{dict()}"""
  |      ^^^^^^ C408
  |
a.py:6:5: F821 Undefined name `x`
  |
6 | rf"{x}"
  |     ^ F821
  |
a.py:8:7: F821 Undefined name `x`
  |
8 | rf"""{x}"""
  |       ^ F821
  |
a.py:11:2: F821 Undefined name `x`
   |
11 | {x}
   |  ^ F821
   |
a.py:14:6: F821 Undefined name `x`
   |
14 | f"\n{x}"
   |      ^ F821
   |
a.py:16:10: F821 Undefined name `x`
   |
16 | f"\u1234{x}"
   |          ^ F821
   |
a.py:18:6: F821 Undefined name `x`
   |
18 | f"\\{x}"
   |      ^ F821
   |
a.py:20:14: F821 Undefined name `x`
   |
20 | f"\\\\\\\\\\{x}"
   |              ^ F821
   |
a.py:23:2: F821 Undefined name `x`
   |
23 | {x}"
   |  ^ F821
   |
Found 11 error(s).
2 potentially fixable with the --fix option.
```

---

_Comment by @charliermarsh on 2022-12-31 04:14_

What?!?!? Noooo way!

---

_Comment by @charliermarsh on 2022-12-31 04:15_

How the heck did you do this? That looks like a really impressive change.

---

_Comment by @harupy on 2022-12-31 04:20_

What I did was the following:

1. Use the right offset when parsing expressions within f-strings.
2. Fix the lexer to preserve the original string to compute the right offset.

---

_Review comment by @harupy on `src/pyupgrade/plugins/native_literals.rs`:56 on 2022-12-31 04:23_

`Tok::Bytes` has been removed. The new lexer only uses `Tok::String`.

Old lexer:

```python
# Implicitly concatenated string
b"" ""  # Tok::Bytes, Tok::String
```

New lexer:

```python
b"" ""   # Tok::String (with kind=Bytes), Tok::String (kind=String)
```

---

_@harupy reviewed on 2022-12-31 04:23_

---

_Review comment by @harupy on `src/pyflakes/snapshots/ruff__pyflakes__tests__F831_F831.py.snap`:5 on 2022-12-31 04:27_

Duplicate function arguments like `def f(a, a): pass` now yield `SyntaxError` ( https://github.com/RustPython/RustPython/pull/4380).

---

_@harupy reviewed on 2022-12-31 04:27_

---

_@harupy reviewed on 2022-12-31 04:27_

---

_Review comment by @harupy on `src/pyflakes/snapshots/ruff__pyflakes__tests__F831_F831.py.snap`:5 on 2022-12-31 04:27_

<img width="1100" alt="image" src="https://user-images.githubusercontent.com/17039389/210124730-2b25cdd2-d849-4fc3-988d-1786646379cc.png">


---

_@harupy reviewed on 2022-12-31 04:29_

---

_Review comment by @harupy on `src/pyflakes/snapshots/ruff__pyflakes__tests__F831_F831.py.snap`:5 on 2022-12-31 04:29_

Maybe we should remove F831 because it's never raised.

---

_@harupy reviewed on 2022-12-31 04:37_

---

_Review comment by @harupy on `src/pyflakes/snapshots/ruff__pyflakes__tests__F831_F831.py.snap`:5 on 2022-12-31 04:37_

or we can just keep it as it's not harmful.

---

_@charliermarsh reviewed on 2022-12-31 04:39_

---

_Review comment by @charliermarsh on `src/pyflakes/snapshots/ruff__pyflakes__tests__F831_F831.py.snap`:5 on 2022-12-31 04:39_

Ah yeah, we can remove in a separate PR.

(Is this true in all Python versions 3.7+? If you know.)

---

_@harupy reviewed on 2022-12-31 04:42_

---

_Review comment by @harupy on `src/pyflakes/snapshots/ruff__pyflakes__tests__F831_F831.py.snap`:5 on 2022-12-31 04:42_

@charliermarsh 

```shell
> docker run python:3.7 python -c "def f(a, a): pass"
  File "<string>", line 1
SyntaxError: duplicate argument 'a' in function definition

> docker run python:3.8 python -c "def f(a, a): pass"
  File "<string>", line 1
SyntaxError: duplicate argument 'a' in function definition

> docker run python:3.9 python -c "def f(a, a): pass"
  File "<string>", line 1
SyntaxError: duplicate argument 'a' in function definition

> docker run python:3.10 python -c "def f(a, a): pass"
  File "<string>", line 1
SyntaxError: duplicate argument 'a' in function definition
```

---

_Comment by @charliermarsh on 2022-12-31 04:43_

This also seems to be a little bit faster...

---

_Merged by @charliermarsh on 2022-12-31 04:43_

---

_Closed by @charliermarsh on 2022-12-31 04:43_

---

_Comment by @charliermarsh on 2022-12-31 04:44_

Honestly this makes me so happy. This is great. Thank you.

---

_Review comment by @harupy on `src/pyflakes/snapshots/ruff__pyflakes__tests__F831_F831.py.snap`:5 on 2022-12-31 04:49_

@charliermarsh 

> we can remove in a separate PR.

I'll file a PR.

---

_@harupy reviewed on 2022-12-31 04:49_

---
