---
number: 15314
title: "`logging-too-many-args (PLE1205)` isn't detected when initialized logger is imported from another module"
type: issue
state: closed
author: osauldmy
labels:
  - question
assignees: []
created_at: 2025-01-07T09:31:36Z
updated_at: 2025-01-07T09:52:59Z
url: https://github.com/astral-sh/ruff/issues/15314
synced_at: 2026-01-07T13:12:16-06:00
---

# `logging-too-many-args (PLE1205)` isn't detected when initialized logger is imported from another module

---

_Issue opened by @osauldmy on 2025-01-07 09:31_

When `logger` isn't initialized in the module, but imported from another module like a singleton (which may be an antipattern anyway), `PLE1205` isn't detected.

Minimal reproducible example:

```python
# direct.py
import logging

logger = logging.getLogger(__name__)

logger.info("Foo", "bar")
```

```python
# foo/logger.py
import logging

logger = logging.getLogger(__name__)
```

```python
# imported.py
from foo.logger import logger

logger.info("Foo", "bar")
```

```shell
$ ruff check --output-format=concise --select PLE
direct.py:5:1: PLE1205 Too many arguments for `logging` format string
Found 1 error.
```

Expected behaviour:

```shell
$ ruff check --output-format=concise --select PLE
direct.py:5:1: PLE1205 Too many arguments for `logging` format string
imported.py:3:1: PLE1205 Too many arguments for `logging` format string
Found 2 errors.
```


---

_Comment by @MichaReiser on 2025-01-07 09:34_

This is a known limitation of Ruff today because it doesn't support multifile analysis. You can work around it by adding `foo.logger.logger` to [`logger-objects`](https://docs.astral.sh/ruff/settings/#lint_logger-objects)

---

_Label `question` added by @MichaReiser on 2025-01-07 09:34_

---

_Comment by @osauldmy on 2025-01-07 09:52_

I completely forgot about `logger-objects` option. Thanks!

---

_Closed by @osauldmy on 2025-01-07 09:52_

---
