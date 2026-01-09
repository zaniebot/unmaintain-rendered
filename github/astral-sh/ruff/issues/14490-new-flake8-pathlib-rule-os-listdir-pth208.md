---
number: 14490
title: "New `flake8-pathlib` rule: `os.listdir` (PTH208)"
type: issue
state: closed
author: sbrugman
labels:
  - rule
assignees: []
created_at: 2024-11-20T14:53:07Z
updated_at: 2024-11-27T09:53:14Z
url: https://github.com/astral-sh/ruff/issues/14490
synced_at: 2026-01-07T13:12:16-06:00
---

# New `flake8-pathlib` rule: `os.listdir` (PTH208)

---

_Issue opened by @sbrugman on 2024-11-20 14:53_

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

The pathlib rules supported by `ruff` could be extended with detection with `os.listdir`:

```python
import os
from pathlib import Path

p = Path(".")
print(os.listdir(p))
```

use instead:

```python
from pathlib import Path

p = Path(".")
print(list(p.iterdir()))
```

The [plugin](https://gitlab.com/RoPP/flake8-use-pathlib) has not been updated for two years. We've extended the plugin rules before using the `PTH2XX` code.


---

_Label `rule` added by @AlexWaygood on 2024-11-20 15:35_

---

_Comment by @MichaReiser on 2024-11-20 17:34_

What's the motivation for the rule? The `os.listdir` code is more concise :)

---

_Comment by @sbrugman on 2024-11-20 18:22_

In isolation there is not much benefit. Using methods on `pathlib.Path` return paths, which have properties and methods for writing concise path operations (`stem`, `suffix`, `parent` etc.)

Best illustrated with real-world use cases: https://github.com/pypa/pipenv/blob/main/tasks/vendoring/__init__.py#L162 or https://github.com/princeton-vl/infinigen/blob/main/infinigen/tools/datarelease_toolkit.py#L354

---

_Referenced in [astral-sh/ruff#14509](../../astral-sh/ruff/pulls/14509.md) on 2024-11-21 10:32_

---

_Closed by @MichaReiser on 2024-11-27 09:53_

---
