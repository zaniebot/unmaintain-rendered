```yaml
number: 9013
title: SLF001 and type annotations
type: issue
state: closed
author: ofek
labels:
  - question
assignees: []
created_at: 2023-12-05T20:08:48Z
updated_at: 2023-12-07T03:08:24Z
url: https://github.com/astral-sh/ruff/issues/9013
synced_at: 2026-01-10T11:09:51Z
```

# SLF001 and type annotations

---

_Issue opened by @ofek on 2023-12-05 20:08_

Should this trigger for type annotations?

```python
from __future__ import annotations

import argparse
from typing import Any

def build_command(subparsers: argparse._SubParsersAction, defaults: Any) -> None:
    ...
```

---

_Comment by @charliermarsh on 2023-12-07 02:06_

That is an interesting question... My initial reaction was "Yes, it should trigger", since it still represents an access on a private API. But I can see why this would be necessary in the ordinary course of business (calling public methods that return opaque private types). So perhaps it should be allowed.

(I kind of feel like such objects should be public, with private interfaces as in, e.g., https://bugs.python.org/issue41592, but recognize that's not within the client's control.)

---

_Label `question` added by @charliermarsh on 2023-12-07 02:07_

---

_Comment by @ofek on 2023-12-07 02:27_

Yeah your assessment is exactly correct, public APIs often do not have perfect encapsulation and expose private details.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-07 02:52_

---

_Closed by @charliermarsh on 2023-12-07 03:05_

---

_Comment by @ofek on 2023-12-07 03:08_

Thank you very much!

---
