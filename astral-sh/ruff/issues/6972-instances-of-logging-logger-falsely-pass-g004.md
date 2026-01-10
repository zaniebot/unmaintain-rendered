---
number: 6972
title: Instances of logging.Logger falsely pass G004 when imported from another module
type: issue
state: closed
author: TimekillerTK
labels:
  - needs-info
assignees: []
created_at: 2023-08-29T11:28:40Z
updated_at: 2023-08-30T08:12:18Z
url: https://github.com/astral-sh/ruff/issues/6972
synced_at: 2026-01-10T01:22:46Z
---

# Instances of logging.Logger falsely pass G004 when imported from another module

---

_Issue opened by @TimekillerTK on 2023-08-29 11:28_



`ruff` does not find errors of type [logging-f-string (G004)](https://beta.ruff.rs/docs/rules/logging-f-string/) when checking a `Logger` object passed as a variable from another module.

* A minimal code snippet that reproduces the bug.

`./main.py`

```py
from .logger import log

log.info(f"{__name__}")
```

`./logger.py`

```py
import logging


def initialize_logger() -> logging.Logger:
    logger = logging.getLogger()
    return logger


log = initialize_logger()

log.info(f"{__name__}")
```

---

* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.

`ruff` will only detect the issue in `./logger.py`, while it will pass `./main.py`.

```sh
$ ruff check --isolated --select G004 src/logger.py

src/logger.py:11:10: G004 Logging statement uses f-string
Found 1 error.

$ ruff check --isolated --select G004 src/main.py

```

`pylint` will find both issues.

```sh
$ pylint src/logger.py

************* Module src.logger
src/logger.py:11:0: W1203: Use lazy % formatting in logging functions (logging-fstring-interpolation)

$ pylint src/main.py

************* Module src.main
src/main.py:3:0: W1203: Use lazy % formatting in logging functions (logging-fstring-interpolation)
```

---

* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).

```sh
$ ruff --version

ruff 0.0.286
```


---

_Comment by @charliermarsh on 2023-08-29 13:42_

Can you take a look at [`logger-objects`](https://beta.ruff.rs/docs/settings/#logger-objects)? Ruff doesn't support resolving symbols across files, so if you re-export a common logger object, we require that you mark it as such (to avoid false positives on non-loggers). In this case, something like:

```toml
[tool.ruff]
logger-objects = ["logger.log"]
```

---

_Label `waiting-on-author` added by @charliermarsh on 2023-08-29 13:42_

---

_Comment by @TimekillerTK on 2023-08-30 08:12_

That works! Adding the following to `pyproject.toml`:
```toml
[tool.ruff]
logger-objects = ["logger.log"]
```

Resulted in:
```sh
$ ruff check --select G004 src/.
src/logger.py:11:13: G004 Logging statement uses f-string
src/main.py:3:13: G004 Logging statement uses f-string
Found 2 errors.
```

---

_Closed by @TimekillerTK on 2023-08-30 08:12_

---
