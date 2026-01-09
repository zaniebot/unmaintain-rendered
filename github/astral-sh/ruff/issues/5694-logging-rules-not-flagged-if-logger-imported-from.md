---
number: 5694
title: Logging rules not flagged if logger imported from another module
type: issue
state: closed
author: petermattia
labels:
  - bug
assignees: []
created_at: 2023-07-11T19:25:59Z
updated_at: 2023-07-24T04:38:21Z
url: https://github.com/astral-sh/ruff/issues/5694
synced_at: 2026-01-07T13:12:15-06:00
---

# Logging rules not flagged if logger imported from another module

---

_Issue opened by @petermattia on 2023-07-11 19:25_

I'm observing that logging rules aren't flagged if a logger is imported from another module.

* A minimal code snippet that reproduces the bug.

If I set up logging here, both of these violations are flagged:
````python
# logging_setup.py
import logging

linter = "ruff"
logging.info(f"Hello, {linter}!") # G004 flagged

logger = logging.getLogger(__name__)

logger.info(f"Hello, {linter}!") # G004 flagged
````

However, if I import `logger` into another module, the below identical violation is not flagged:
````python
# my_module.py
from logging_setup import logger

linter = "ruff"
logger.info(f"Hello, {linter}!") # G004 *not* flagged
````

* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.

`ruff .`

* The current Ruff settings (any relevant sections from your `pyproject.toml`).

`"G"` ruleset

* The current Ruff version (`ruff --version`).
`0.0.277`


---

_Comment by @petermattia on 2023-07-13 22:08_

Hi `ruff` team, friendly follow up on this one, please let me know if I'm missing something here. Thanks!

---

_Comment by @charliermarsh on 2023-07-13 22:29_

Will take a look.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-07-13 22:35_

---

_Comment by @charliermarsh on 2023-07-13 22:35_

Looks like an oversight, we would flag `logging_setup.logger.info(f"Hello, {linter}!")` but not the `import from` variant.

---

_Label `bug` added by @charliermarsh on 2023-07-13 22:35_

---

_Comment by @petermattia on 2023-07-13 22:41_

Thanks @charliermarsh, that makes sense.

---

_Comment by @charliermarsh on 2023-07-13 22:53_

I think I misdiagnosed this, `logging_setup.logger.info` also isn't working in my initial testing, but regardless it'll be fixed in the next release.

---

_Referenced in [astral-sh/ruff#5750](../../astral-sh/ruff/pulls/5750.md) on 2023-07-13 23:03_

---

_Closed by @charliermarsh on 2023-07-24 04:38_

---
