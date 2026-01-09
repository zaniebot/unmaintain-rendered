---
number: 9873
title: How could I disable autoremove unused import?
type: issue
state: closed
author: ErnestDong
labels:
  - question
assignees: []
created_at: 2024-02-07T15:12:14Z
updated_at: 2024-02-08T02:50:26Z
url: https://github.com/astral-sh/ruff/issues/9873
synced_at: 2026-01-07T13:12:15-06:00
---

# How could I disable autoremove unused import?

---

_Issue opened by @ErnestDong on 2024-02-07 15:12_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
I usually write python code in vscode as follows:
```python
#%%
import requests
import pandas as pd

res=requests.get(...)
```

where `#%%` creates a new notebook cell to run, `pandas` will be used to parse request JSON.

But Ruff will auto-remove unused imports and delete the `pandas` import. It is a burden to my mind that I need to use the temporary unused `pandas` before saving the file. How to fix that?

---

_Comment by @charliermarsh on 2024-02-07 16:13_

You can mark the rule as unfixable in your Ruff configuration. For example, create a `ruff.toml` with...

```toml
[lint]
# Disable fix for unused imports (`F401`).
unfixable = ["F401"]
```

---

_Label `question` added by @charliermarsh on 2024-02-07 16:13_

---

_Comment by @ErnestDong on 2024-02-08 01:56_

Thank you

---

_Closed by @ErnestDong on 2024-02-08 01:56_

---

_Comment by @charliermarsh on 2024-02-08 02:50_

No prob!

---
