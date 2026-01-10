```yaml
number: 5086
title: "Formatter: Trailing elif comments disappear"
type: issue
state: closed
author: konstin
labels:
  - bug
  - formatter
assignees: []
created_at: 2023-06-14T15:03:43Z
updated_at: 2023-06-29T18:54:44Z
url: https://github.com/astral-sh/ruff/issues/5086
synced_at: 2026-01-10T11:09:47Z
```

# Formatter: Trailing elif comments disappear

---

_Issue opened by @konstin on 2023-06-14 15:03_

Running the formatter on 
```python
if 1:
    pass
elif 2:
    pass
# mid comment
elif 3:
    pass
# Trailing elif comment
if True:
    pass
```
will print
```python
if 1:
    pass
elif 2:
    pass
# mid comment
elif 3:
    pass
if True:
    pass
```
The trailing comment on  the last elif is lost, but it should have been preserved.

Minimized from https://github.com/python/cpython/blob/7199584ac8632eab57612f595a7162ab8d2ebbc0/Lib/dataclasses.py#L494-L543


---

_Label `bug` added by @konstin on 2023-06-14 15:03_

---

_Label `autoformatter` added by @konstin on 2023-06-14 15:03_

---

_Comment by @qdegraaf on 2023-06-29 18:18_

Is this still an issue? Was looking at this as a way to get into the formatter but after making a scratch file with the snippet and running
```
cargo run --bin ruff_python_formatter -- --emit stdout scratch.py
```
I get a correct result 
```
Finished dev [unoptimized + debuginfo] target(s) in 0.18s
     Running `target/debug/ruff_python_formatter --emit stdout scratch.py`
if 1:
    pass
elif 2:
    pass
# mid comment
elif 3:
    pass
# Trailing elif comment
if True:
    pass
```

Adding various bits of whitespace and moving some statements around all seem alright. Ran it on the CPython snippet as well and don't see any comments disappear either. 

---

_Comment by @konstin on 2023-06-29 18:42_

sorry, i forgot to close the issue after fixing it in #5210

btw i'm currently doing a big refactoring on `StmtIf` (https://github.com/astral-sh/ruff/compare/main...refactor_stmt_if), which will change most of this logic again since it introduces dedicates nodes for `elif` and `else` of `if` statements

---

_Closed by @konstin on 2023-06-29 18:42_

---

_Comment by @qdegraaf on 2023-06-29 18:54_

All good! Thanks for the info, I'll find something else!

---
