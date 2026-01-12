```yaml
number: 16359
title: Combination of re-exported import and classvar of the same name results in F811
type: issue
state: closed
author: thatch
labels:
  - bug
assignees: []
created_at: 2025-02-25T00:54:40Z
updated_at: 2025-02-25T08:24:15Z
url: https://github.com/astral-sh/ruff/issues/16359
synced_at: 2026-01-12T15:54:55Z
```

# Combination of re-exported import and classvar of the same name results in F811

---

_@thatch_

### Description

demo.py

```py
from msgspec import Struct
from msgspec.structs import replace as replace


class T(Struct):
    search: str = ""
    replace: str = ""
```

However running:

```
$ uvx ruff check --fix demo.py
demo.py:7:5: F811 Redefinition of unused `replace` from line 2
  |
5 | class T(Struct):
6 |     search: str = ""
7 |     replace: str = ""
  |     ^^^^^^^ F811
  |
  = help: Remove definition: `replace`

Found 1 error.
```

I don't think this should be treated as shadowing (it only is during the class body).  Adding a module-level use of the re-exported `replace` makes it chill out, but I couldn't figure out a good way to use the classvar to do so.

```
$ uvx ruff --version
ruff 0.9.7
```

---

_Label `bug` added by @ntBre on 2025-02-25 02:41_

---

_Comment by @MichaReiser on 2025-02-25 08:23_

This might be a duplicate of https://github.com/astral-sh/ruff/issues/10874

---

_Comment by @MichaReiser on 2025-02-25 08:24_

Yeah, I think this is a duplicate (see the very last comment in that thread). 

Thanks for reporting this issue. I'll merge it with the existing issue.

---

_Closed by @MichaReiser on 2025-02-25 08:24_

---
