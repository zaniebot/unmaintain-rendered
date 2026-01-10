```yaml
number: 9156
title: "PYI055's \"safe\" fix breaks type annotation semantics"
type: issue
state: closed
author: tooruu
labels:
  - bug
assignees: []
created_at: 2023-12-16T02:50:58Z
updated_at: 2023-12-16T20:58:29Z
url: https://github.com/astral-sh/ruff/issues/9156
synced_at: 2026-01-10T11:09:51Z
```

# PYI055's "safe" fix breaks type annotation semantics

---

_Issue opened by @tooruu on 2023-12-16 02:50_

Ruff 0.1.8
`ruff check --isolated --select PYI055 --fix`
turns
```py
def convert_union(union: UnionType) -> _T | None:
    converters: tuple[
        type[_T] | type[Converter[_T]] | Converter[_T] | Callable[[str], _T], ...
    ] = union.__args__
    ...
```
into
```py
def convert_union(union: UnionType) -> _T | None:
    converters: tuple[
        type[_T | Converter[_T]], ...
    ] = union.__args__
    ...
```
completely disregarding `Converter[_T] | Callable[[str], _T]`

---

_Label `bug` added by @zanieb on 2023-12-16 03:05_

---

_Comment by @zanieb on 2023-12-16 03:06_

Thanks for the report! This just looks like a bug cc @diceroll123 

ref https://github.com/astral-sh/ruff/pull/7886 https://github.com/astral-sh/ruff/pull/7934

---

_Comment by @diceroll123 on 2023-12-16 12:14_

Sorry about that silly oversight! Opened a PR to fix.

---

_Closed by @charliermarsh on 2023-12-16 20:58_

---
