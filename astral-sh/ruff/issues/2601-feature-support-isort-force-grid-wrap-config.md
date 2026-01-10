---
number: 2601
title: "Feature: Support isort force_grid_wrap config"
type: issue
state: open
author: mikelane
labels:
  - isort
assignees: []
created_at: 2023-02-06T01:49:33Z
updated_at: 2024-09-23T20:36:39Z
url: https://github.com/astral-sh/ruff/issues/2601
synced_at: 2026-01-10T01:22:41Z
---

# Feature: Support isort force_grid_wrap config

---

_Issue opened by @mikelane on 2023-02-06 01:49_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

The isort library has a setting that will "force number of from imports to be grid wrapped regardless of line length." Using this option will make it so the users can configure how the imports will wrap when using grid mode. This is particularly useful for when it is desirable to have every import on a unique line (which makes it much easier to identify what has changed in a PR, for example).

Ideally this would follow the isort setting:

```toml
[tool.ruff.isort]
force_grid_wrap = 2  # Wrap from imports starting at the 2nd import
```

When coupled with the `multi_line_output = 3` mode, for example, imports that start as this:

```python
from itertools import permutations, combinations
```

become this:

```python
from itertools import (
    combinations,
    permutations,
)
```

---

_Label `isort` added by @charliermarsh on 2023-02-06 02:04_

---

_Comment by @spaceone on 2023-02-08 12:30_

See also #2600

---

_Referenced in [astral-sh/ruff#2595](../../astral-sh/ruff/issues/2595.md) on 2023-05-10 05:16_

---

_Referenced in [astral-sh/ruff#6190](../../astral-sh/ruff/issues/6190.md) on 2023-07-31 09:46_

---

_Referenced in [ChildMindInstitute/mindlogger-backend-refactor#1034](../../ChildMindInstitute/mindlogger-backend-refactor/pulls/1034.md) on 2024-01-25 02:16_

---

_Referenced in [ChildMindInstitute/mindlogger-backend-refactor#1035](../../ChildMindInstitute/mindlogger-backend-refactor/pulls/1035.md) on 2024-01-25 13:45_

---

_Comment by @msto on 2024-09-23 20:36_

Would love to see this implemented!

---

_Referenced in [astral-sh/ruff#2600](../../astral-sh/ruff/issues/2600.md) on 2025-06-02 02:03_

---

_Referenced in [devitocodes/devito#2771](../../devitocodes/devito/pulls/2771.md) on 2025-10-17 12:47_

---
