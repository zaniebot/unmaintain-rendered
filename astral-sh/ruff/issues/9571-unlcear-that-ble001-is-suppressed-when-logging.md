```yaml
number: 9571
title: Unlcear that BLE001 is suppressed when logging/raising error.
type: issue
state: closed
author: ollz272
labels:
  - documentation
assignees: []
created_at: 2024-01-18T09:41:05Z
updated_at: 2024-01-19T16:58:33Z
url: https://github.com/astral-sh/ruff/issues/9571
synced_at: 2026-01-12T15:54:49Z
```

# Unlcear that BLE001 is suppressed when logging/raising error.

---

_@ollz272_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

The documentation isn't clear when BLE001 gets suppressed. Looking at the following code:

```python
"""TEST."""
import logging

log = logging.getLogger(__name__)


def my_func() -> None:  # noqa: D103
    pass


try:
    my_func()
except Exception:
    log.exception("hello")

try:
    my_func()
except Exception:
    my_func()
    raise

try:
    my_func()
except Exception:
    my_func()
```

Only the last try statement gets picked up as BLE001. So im guessing the following rules apply:
- If we log the exception in the except block, dont raise BLE001
- If we raise the exception in the except block, dont raise BLE001

However, this isn't clear from the documentation - it just implies bare excepts will cause it. Can this be updated so we understand the rules better, please?


---

_Label `documentation` added by @charliermarsh on 2024-01-18 15:10_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-19 16:43_

---

_Closed by @charliermarsh on 2024-01-19 16:58_

---
