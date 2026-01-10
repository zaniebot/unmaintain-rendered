---
number: 20839
title: "F401: false positive causes NameError"
type: issue
state: closed
author: spaceone
labels:
  - bug
  - preview
assignees: []
created_at: 2025-10-13T09:06:23Z
updated_at: 2025-10-27T15:23:38Z
url: https://github.com/astral-sh/ruff/issues/20839
synced_at: 2026-01-10T01:23:01Z
---

# F401: false positive causes NameError

---

_Issue opened by @spaceone on 2025-10-13 09:06_

### Summary

ruff 0.14.0 introduced a false-positive for F401 unused-import:

```python
  try:
      import asdfasfd.asdf.sadf
  except ImportError:
      import argparse
      import asdfasfd  # W: `asdfasfd` imported but unused
      asdfasfd.asdf = argparse.Namespace()
```

autofix causes:
```python
Traceback (most recent call last):                                                                                                                                                                                                             
  File "foo.py", line 6, in <module>                                                
    asdfasfd.asdf = argparse.Namespace()       
    ^^^^^^^^                                               
NameError: name 'asdfasfd' is not defined   
```

### Version

0.14.0

---

_Comment by @ntBre on 2025-10-13 12:47_

cc @dylwil3 

This is interesting, I would have assumed that the first import would still import `asdfasfd` even if a submodule import fails. Maybe that's not the case, or maybe something else is going wrong.

---

_Label `preview` added by @ntBre on 2025-10-13 12:47_

---

_Label `bug` added by @ntBre on 2025-10-13 12:47_

---

_Comment by @dylwil3 on 2025-10-13 17:54_

Interesting! If you look at the bytecode, Python only stores the symbol for the module if the full import succeeds:

```pycon
>>> def func():
...       try:
...          import asdfasfd.asdf.sadf
...       except ImportError:
...          import argparse
...          import asdfasfd  # W: `asdfasfd` imported but unused
...          asdfasfd.asdf = argparse.Namespace()
...
>>> dis.dis(func)
   1           RESUME                   0

   2           NOP

   3   L1:     LOAD_SMALL_INT           0
               LOAD_CONST               1 (None)
               IMPORT_NAME              0 (asdfasfd.asdf.sadf)
               STORE_FAST               0 (asdfasfd)
       L2:     LOAD_CONST               1 (None)
               RETURN_VALUE

< rest omitted >
```

I'd have to think about how to detect situations like this... I guess more generally the presence of control flow would likely confuse the preview behavior.

---

_Assigned to @dylwil3 by @MichaReiser on 2025-10-14 06:34_

---

_Referenced in [astral-sh/ruff#20878](../../astral-sh/ruff/pulls/20878.md) on 2025-10-15 01:25_

---

_Closed by @dylwil3 on 2025-10-27 15:23_

---
