```yaml
number: 2681
title: "PLE2502: Location of NoQA comment"
type: issue
state: closed
author: AA-Turner
labels:
  - bug
assignees: []
created_at: 2023-02-09T03:32:22Z
updated_at: 2023-02-10T21:59:30Z
url: https://github.com/astral-sh/ruff/issues/2681
synced_at: 2026-01-12T15:54:43Z
```

# PLE2502: Location of NoQA comment

---

_@AA-Turner_

This one is slightly more complicated to report as it has actual embedded RTL/LTR marks.

```python
from pathlib import Path
Path("bug.py").write_bytes(b'text = ("spam\\n"\r\n        "eggs\\n"\r\n        "fan \xe2\x80\x8f\xd7\xa2\xd7\x91\xd7\xa8\xd7\x99\xd7\xaa\xe2\x80\x8e\\n"  # NoQA: PLE2502\r\n        "man\\n")\r\n')
```

Running ``ruff --isolated --select PLE2502 bug.py`` with the ``NoQA`` comment on line 3 (as in the initial example) doesn't silence the error. Moving the comment to line 1 does silence the error -- perhaps something is off with how implicit concatenation is handled?

(reproducer that silences the error, note that the NoQA comment has moved to line 1):

```python
from pathlib import Path
Path("bug.py").write_bytes(b'text = ("spam\\n"  # NoQA: PLE2502\r\n        "eggs\\n"\r\n        "fan \xe2\x80\x8f\xd7\xa2\xd7\x91\xd7\xa8\xd7\x99\xd7\xaa\xe2\x80\x8e\\n"\r\n        "man\\n")\r\n')
```

Copying @colin99d as the author of the feature.

A

---

_Label `bug` added by @charliermarsh on 2023-02-09 03:34_

---

_Closed by @charliermarsh on 2023-02-10 21:59_

---
