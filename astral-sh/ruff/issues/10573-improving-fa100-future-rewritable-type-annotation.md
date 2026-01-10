```yaml
number: 10573
title: "Improving \"FA100 / future-rewritable-type-annotation\" message"
type: issue
state: closed
author: Avasam
labels:
  - rule
  - help wanted
assignees: []
created_at: 2024-03-25T16:41:11Z
updated_at: 2024-05-13T01:38:51Z
url: https://github.com/astral-sh/ruff/issues/10573
synced_at: 2026-01-10T11:09:52Z
```

# Improving "FA100 / future-rewritable-type-annotation" message

---

_Issue opened by @Avasam on 2024-03-25 16:41_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

The message for rule https://docs.astral.sh/ruff/rules/future-rewritable-type-annotation/ `Missing from __future__ import annotations, but uses {name}` is slightly confusing / misleading. It sounds like I need to use `from __future__ import annotations` when using `typing.Union`, when it actually means that adding `from __future__ import annotations` *can* lead to simplified /modernized syntax. But that's also not checked unless relevant pyupgrades rules are turned on.

I think the message could be tweaked a bit to reflect that. I also assume FA and UP rules will likely be grouped together after the rules categorization #1774




---

_Label `rule` added by @AlexWaygood on 2024-03-25 18:25_

---

_Label `help wanted` added by @MichaReiser on 2024-03-26 07:46_

---

_Comment by @Olegt0rr on 2024-04-11 14:49_

The same issue.
`FA100` false positive detection

python 3.9
target-version = "py39"

```python
from typing import Optional

from aiohttp import ClientSession

class Client:
    def __init__(self) -> None:
        self._session: Optional[ClientSession] = None
```

`Optional` is highlighted with 
```
FA100 Missing `from __future__ import annotations`, but uses `typing.Optional`
```

---

_Closed by @charliermarsh on 2024-05-13 01:38_

---
