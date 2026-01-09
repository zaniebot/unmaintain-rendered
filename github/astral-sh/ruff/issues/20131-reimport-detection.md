---
number: 20131
title: Reimport detection
type: issue
state: closed
author: spaceby
labels: []
assignees: []
created_at: 2025-08-28T10:58:02Z
updated_at: 2025-08-28T13:08:32Z
url: https://github.com/astral-sh/ruff/issues/20131
synced_at: 2026-01-07T13:12:16-06:00
---

# Reimport detection

---

_Issue opened by @spaceby on 2025-08-28 10:58_

### Summary

I'm interested in the situation where a method is imported from a file that imports it from another file

For example

file_with_function.py
```python
def foo():
    pass
```
---
file_with_import.py
```python
from file_with_function import foo
```
---
main.py
```python
from file_with_import import foo
```
---

Reimports are hard to spot with the naked eye. They can't be found by searching. And they can cause the hard-to-detect circular import problem.

A rule to correct this would be useful.

---

_Comment by @ntBre on 2025-08-28 13:08_

I think this is the same as #19559, so I'll close this as a duplicate. Thanks for the suggestion!

https://github.com/astral-sh/ruff/issues/20092 is another related issue

---

_Closed by @ntBre on 2025-08-28 13:08_

---
