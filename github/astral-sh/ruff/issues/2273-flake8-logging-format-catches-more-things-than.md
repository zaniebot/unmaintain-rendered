---
number: 2273
title: flake8-logging-format catches more things than ruff
type: issue
state: closed
author: alexprengere
labels: []
assignees: []
created_at: 2023-01-27T18:38:33Z
updated_at: 2023-02-03T13:12:44Z
url: https://github.com/astral-sh/ruff/issues/2273
synced_at: 2026-01-07T13:12:14-06:00
---

# flake8-logging-format catches more things than ruff

---

_Issue opened by @alexprengere on 2023-01-27 18:38_

Tested with ruff `0.0.236` and the latest flake8-logging-format. This is an error (G004) with flake8 but passes with ruff:

```python
import logging
log = logging.getLogger(__name__)
msg = "message"
log.warning(f"A {msg}")
```

When using directly `logging.warning`, both fail.
Using `logging.getLogger` is a common idiom when logging messages on large projects, so it would be super useful to catch it as well.


---

_Comment by @edgarrmondragon on 2023-01-27 19:06_

@alexprengere can you try calling it `logger` instead of `log`?

```python
import logging
logger = logging.getLogger(__name__)
msg = "message"
logger.warning(f"A {msg}")
```

`flake8-logging-format` only excludes the `warnings.` and `parser.` call paths, so `log.warning` is identified as a logger call. However, `ruff` only _includes_ `logging` and `*logger` call paths, so `log.warning` is not identified as a logger.

This is probably easy to fix by changing `is_logger_candidate` to check for `*log`: https://github.com/charliermarsh/ruff/blob/3ec46f093604c4fe79f724a5f927b241387a239c/src/ast/helpers.rs#L1009-L1019

Doing the same as `flake8-logging-format` is probably prone to a lot of false positives if an arbitrary API implements an `error` method, for example.

---

_Comment by @charliermarsh on 2023-01-27 20:34_

Yeah we could extend to anything that includes log, I think.

---

_Comment by @charliermarsh on 2023-01-27 20:38_

(It’s also possible that I was wrong to restrict the checks in this way.)

---

_Comment by @Flowake on 2023-01-27 22:42_

The issue of correctly identifying how loggers are named arises for multiple rules.
Maybe we could define in one place the accepted names for some object to be considered a logger ? (I'm thinking about TRY400 which is only triggered by the logger being called "logging" or "logger")

---

_Comment by @charliermarsh on 2023-01-27 23:00_

We do have that centralized helper (`is_logger_candidate`), which TRY400 uses too. So if we just tweak that, it'll fix this for both rules.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-27 23:32_

---

_Referenced in [astral-sh/ruff#2279](../../astral-sh/ruff/pulls/2279.md) on 2023-01-27 23:35_

---

_Closed by @charliermarsh on 2023-01-27 23:41_

---

_Comment by @alexprengere on 2023-02-03 13:12_

Thanks a lot for the quick fix!

---
