```yaml
number: 1187
title: Upgrade RustPython to fix end location of implicitly concatenated strings
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: upgrade-RustPython
created_at: 2022-12-10T23:52:15Z
updated_at: 2022-12-11T02:11:47Z
url: https://github.com/astral-sh/ruff/pull/1187
synced_at: 2026-01-12T05:36:31Z
```

# Upgrade RustPython to fix end location of implicitly concatenated strings

---

_Pull request opened by @harupy on 2022-12-10 23:52_

To include https://github.com/RustPython/RustPython/pull/4324, upgrade RustPython.

## Before

```
> echo 'h = "x" "y" f"z"' | cargo run -- --show-source -
-:1:5: F541 f-string without any placeholders
  |
1 | h = "x" "y" f"z"
  |     ^^^ F541
  |
```

## After

```
> echo 'h = "x" "y" f"z"' | cargo run -- --show-source -
-:1:5: F541 f-string without any placeholders
  |
1 | h = "x" "y" f"z"
  |     ^^^^^^^^^^^^ F541
  |
```

---

_Comment by @charliermarsh on 2022-12-11 00:07_

Oh, awesome.

---

_Comment by @charliermarsh on 2022-12-11 00:12_

Very random, but the worst offender of this kind of thing is end-of-scope:

```py
def f():
    x = 1
















x = 2
```

For example, in this file, RustPython thinks that `f` ends at line 19!


---

_Merged by @charliermarsh on 2022-12-11 00:16_

---

_Closed by @charliermarsh on 2022-12-11 00:16_

---

_Branch deleted on 2022-12-11 00:17_

---

_Comment by @harupy on 2022-12-11 01:26_

@charliermarsh What's the desired end location of `f`? Thats' the end of `x = 1`, right?

---

_Comment by @charliermarsh on 2022-12-11 02:11_

@harupy - Yeah I generally just defer to whatever CPython does:

```py
>>> print(ast.dump(ast.parse(s), include_attributes=True))
Module(body=[FunctionDef(name='f', args=arguments(posonlyargs=[], args=[], kwonlyargs=[], kw_defaults=[], defaults=[]), body=[Assign(targets=[Name(id='x', ctx=Store(), lineno=2, col_offset=4, end_lineno=2, end_col_offset=5)], value=Constant(value=1, lineno=2, col_offset=8, end_lineno=2, end_col_offset=9), lineno=2, col_offset=4, end_lineno=2, end_col_offset=9)], decorator_list=[], lineno=1, col_offset=0, end_lineno=2, end_col_offset=9), Assign(targets=[Name(id='x', ctx=Store(), lineno=19, col_offset=0, end_lineno=19, end_col_offset=1)], value=Constant(value=2, lineno=19, col_offset=4, end_lineno=19, end_col_offset=5), lineno=19, col_offset=0, end_lineno=19, end_col_offset=5)], type_ignores=[])
```

So in this case, `f` starts on line 1, column 0, and ends at line 2, column 9.

---
