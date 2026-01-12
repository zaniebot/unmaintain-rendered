```yaml
number: 17014
title: "E712: auto-fix disagrees with suggested fix in output for equality comparisons to bools"
type: issue
state: closed
author: The-Compiler
labels:
  - fixes
assignees: []
created_at: 2025-03-27T17:01:29Z
updated_at: 2025-04-23T15:49:21Z
url: https://github.com/astral-sh/ruff/issues/17014
synced_at: 2026-01-12T15:54:55Z
```

# E712: auto-fix disagrees with suggested fix in output for equality comparisons to bools

---

_@The-Compiler_

### Summary

With [(playground)](https://play.ruff.rs/21a84874-6424-4d74-9ee1-12d4fc7ddd04):

```python
flag = True

if flag == True:
    pass

if flag == False:
    pass
```

running `ruff check` suggests:

```console
$ ruff check --unsafe-fixes ruff_demo.py
ruff_demo.py:3:4: E712 [*] Avoid equality comparisons to `True`; use `if flag:` for truth checks
  [...]
  = help: Replace with `flag`

ruff_demo.py:6:4: E712 [*] Avoid equality comparisons to `False`; use `if not flag:` for false checks
  [...]
  = help: Replace with `not flag`
```

but `ruff check --fix --unsafe-fixes` results in a different result than what the output suggests, and code that is still unidiomatic:

```python
flag = True

if flag is True:
    pass

if flag is False:
    pass
```

while I would have expected

```python
flag = True

if flag:
    pass

if not flag:
    pass
```

instead. Of course those aren't equivalent in all cases if `flag` wasn't a bool, but the fix is already unsafe anyways, so it might as well end up with the more idiomatic result if manual review is required.

Somewhat related:

- #4560

but once such type-specific fixing exists, the behavior could be improved accordingly:

```python
optional_flag: bool | None = False
if optional_flag == False: ...  # -->
if optional_flag is False: ...

flag: bool = False
if flag == False:  ...  # -->
if not flag: ...
```

### Version

ruff 0.11.2 (4773878ee 2025-03-21)

---

_Label `fixes` added by @ntBre on 2025-03-27 17:05_

---

_Comment by @knavdeep152002 on 2025-03-31 11:00_

I want to take this up, but I have a few doubts about it.

Is the equality comparison to bool only fixed to a single operation like `flag==True`, `flag==False` or can it be something like `flag==True < 5`(weird but possible)? 

The second expression is also valid, but if we convert it to flag < 5 it's not the right equivalent to the prior rather flag is True < 5 might make sense.

So should the fix be supported to a single operator case? 

---

_Closed by @ntBre on 2025-04-23 15:49_

---

_Closed by @ntBre on 2025-04-23 15:49_

---
