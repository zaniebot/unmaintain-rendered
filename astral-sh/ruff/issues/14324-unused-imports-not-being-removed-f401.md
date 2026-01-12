```yaml
number: 14324
title: Unused imports not being removed F401
type: issue
state: closed
author: RossLote
labels:
  - question
assignees: []
created_at: 2024-11-13T18:43:06Z
updated_at: 2024-11-13T19:27:20Z
url: https://github.com/astral-sh/ruff/issues/14324
synced_at: 2026-01-12T15:54:53Z
```

# Unused imports not being removed F401

---

_@RossLote_

```
uv init tester --package --python 3.11
cd tester
```
edit file:
```
import os

def main() -> None:
    print("Hello from tester!")
```
```
uvx ruff check --fix
src/tester/__init__.py:1:8: F401 `os` imported but unused
  |
1 | import os
  |        ^^ F401
2 | 
3 | def main() -> None:
  |
  = help: Remove unused import: `os`

Found 1 error.
```

```
uvx ruff --version
ruff 0.7.3
```

OS: MacOS 15.0.1 


---

_Comment by @charliermarsh on 2024-11-13 19:06_

I think it's because it's an `__init__.py` file specifically. In `--preview`, we offer you that fix as unsafe.

---

_Label `question` added by @charliermarsh on 2024-11-13 19:06_

---

_Comment by @RossLote on 2024-11-13 19:10_

That worked! Thanks for the fast response.

`uvx ruff check --unsafe-fixes --preview --fix`

---

_Closed by @RossLote on 2024-11-13 19:27_

---
