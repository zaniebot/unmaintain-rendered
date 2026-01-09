---
number: 22186
title: This bad python code is passed as ok
type: issue
state: closed
author: Alexey-T
labels: []
assignees: []
created_at: 2025-12-24T23:48:43Z
updated_at: 2025-12-25T09:09:40Z
url: https://github.com/astral-sh/ruff/issues/22186
synced_at: 2026-01-07T13:12:16-06:00
---

# This bad python code is passed as ok

---

_Issue opened by @Alexey-T on 2025-12-24 23:48_

### Summary

example python script in a new tab (filename is empty in my text editor CudaText):
```
import    sys

print '2' sys.path
```

Ruff 0.14.10 Win64. it is run as: 

`c:\Utils\ruff.EXE check --output-format=concise --select E,W,F,B,I --ignore E501,W191,E111 @`

and it cannot find lint-errors here.

- missing brackets near `print` (I use Py 3.8)
- missing operator between `'2'` and `sys.path`

change script to:
```
import    sys

print '2 sys.path
```

- Ruff cannot find UNCLOSED QUOTE.

---

_Comment by @injust on 2025-12-25 06:27_

Can't reproduce on macOS. Ruff 0.14.10 gives me `invalid-syntax` when running `ruff check` on either snippet.

---

_Closed by @Alexey-T on 2025-12-25 08:29_

---

_Comment by @Alexey-T on 2025-12-25 09:09_

my mistake.

---
