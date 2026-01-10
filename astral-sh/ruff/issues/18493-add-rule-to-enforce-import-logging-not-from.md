```yaml
number: 18493
title: Add rule to enforce import logging (not from logging import ...), matching Python documentation best practice
type: issue
state: closed
author: EricWebsmith
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-06-06T09:49:03Z
updated_at: 2025-06-09T03:16:59Z
url: https://github.com/astral-sh/ruff/issues/18493
synced_at: 2026-01-10T11:09:58Z
```

# Add rule to enforce import logging (not from logging import ...), matching Python documentation best practice

---

_Issue opened by @EricWebsmith on 2025-06-06 09:49_

### Summary


Using `import logging` (not `from logging import xxx`) is the preferred way to import the logging module in Python. This is recommended in the [official Python logging documentation](https://docs.python.org/3/howto/logging.html).

I noticed that `flake8-pytest-style` enforces a similar convention for `pytest `via rule `PT013`, requiring `import pytest` instead of `from pytest import xxx` I believe a similar rule would be valuable for logging—either in Ruff or (if revived) flake8-logging.

Feature proposal:

- Add a rule to flag any `from logging import xxx` import and recommend using `import logging` instead.

- This would align Ruff with Python’s official logging guidance and make codebases more consistent and readable.

Example (to be flagged):

```python
from logging import LogRecord, getLogger
```
Recommended:

```python
import logging
```

Thanks for considering this feature!



---

_Label `rule` added by @ntBre on 2025-06-06 16:26_

---

_Label `needs-decision` added by @ntBre on 2025-06-06 16:26_

---

_Comment by @ntBre on 2025-06-06 16:29_

I think this is already covered by [banned-import-from (ICN003)](https://docs.astral.sh/ruff/rules/banned-import-from/#banned-import-from-icn003) with [`lint.flake8-import-conventions.banned-from`](https://docs.astral.sh/ruff/settings/#lint_flake8-import-conventions_banned-from) configured to include `logging`.

---

_Comment by @EricWebsmith on 2025-06-09 03:16_

> I think this is already covered by [banned-import-from (ICN003)](https://docs.astral.sh/ruff/rules/banned-import-from/#banned-import-from-icn003) with [`lint.flake8-import-conventions.banned-from`](https://docs.astral.sh/ruff/settings/#lint_flake8-import-conventions_banned-from) configured to include `logging`.

Thanks ntBre. I added the following config and it worked. 

```toml
[tool.ruff.lint.flake8-import-conventions]
# Declare the banned `from` imports.
banned-from = ["logging"]
```

---

_Closed by @EricWebsmith on 2025-06-09 03:16_

---
