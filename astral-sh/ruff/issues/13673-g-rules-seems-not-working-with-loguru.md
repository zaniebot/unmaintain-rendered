```yaml
number: 13673
title: G rules seems not working with loguru.
type: issue
state: closed
author: qiuxiafei
labels:
  - question
assignees: []
created_at: 2024-10-08T01:21:50Z
updated_at: 2024-10-13T15:16:09Z
url: https://github.com/astral-sh/ruff/issues/13673
synced_at: 2026-01-12T15:54:53Z
```

# G rules seems not working with loguru.

---

_@qiuxiafei_

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

It seems `G` rules only works with built-in `logging` module not loguru. Is it by-design or a bug?

Testing script:
```python
import logging
from loguru import logger

a = 100
logger.info(f"Value of a is {a}")
logging.info(f"Value of a is {a}")

logger.warn("This is a warning")
logging.warn("This is a warning")
```
ruff command (executed in empty project directory without config file) : `ruff check --select G ./test.py`.
The output:
```
  |
4 | a = 100
5 | logger.info(f"Value of a is {a}")
6 | logging.info(f"Value of a is {a}")
  |              ^^^^^^^^^^^^^^^^^^^^ G004
7 |
8 | logger.warn("This is a warning")
  |

test.py:9:9: G010 [*] Logging statement uses `warn` instead of `warning`
  |
8 | logger.warn("This is a warning")
9 | logging.warn("This is a warning")
  |         ^^^^ G010
  |
  = help: Convert to `warning`

Found 2 errors.
[*] 1 fixable with the `--fix` option.
```
ruff points out all errors on logging module but none of loguru's.

ruff version: 0.6.9



---

_Label `rule` added by @MichaReiser on 2024-10-08 07:32_

---

_Comment by @MichaReiser on 2024-10-08 07:36_

Yes, this seems intentional. 

Ruff matches the upstream [`flake8-logging-format`](https://pypi.org/project/flake8-logging-format/) linter and its documentation states that it is specific to Python's `logging` module

> Python [logging](https://docs.python.org/3/library/logging.html#logging.Logger.debug) supports a special extra keyword for passing a dictionary of user-defined attributes to include in a logging event.

Our current focus is on linting vanilla Python code, and we only make an exception for very popular third-party libraries that are considered community standards. 

---

_Label `rule` removed by @MichaReiser on 2024-10-08 07:36_

---

_Label `question` added by @MichaReiser on 2024-10-08 07:36_

---

_Closed by @MichaReiser on 2024-10-13 15:16_

---
