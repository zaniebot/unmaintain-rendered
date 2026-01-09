---
number: 7502
title: Inconsistent logging detection
type: issue
state: closed
author: konstin
labels:
  - rule
assignees: []
created_at: 2023-09-18T20:22:48Z
updated_at: 2023-09-27T00:22:23Z
url: https://github.com/astral-sh/ruff/issues/7502
synced_at: 2026-01-07T13:12:15-06:00
---

# Inconsistent logging detection

---

_Issue opened by @konstin on 2023-09-18 20:22_

There are three ways you can call the logging module:

1.
```python
import logging
logger = logging.getLoger(__name__)
logger.info("msg")
```
2.
```python
import logging
logging.info("msg")
```
3.
```python
from logging import info
info("msg")
```

The logging rules are enforced inconsistently between those three call kinds:

```python
import logging
from logging import warn, info, error

logger = logging.getLogger(__name__)

logging.info(f"{1}")  # G004
logger.info(f"{1}")  # G004
info(f"{1}")

logging.info("", 2)  # PLE1205
logger.info("", 2)  # PLE1205
info("", 2)

logging.warn("a")  # PGH002, G010
logger.warn("a")  # G010
warn("a")  # PGH002

try:
    print(1 / 0)
except Exception:
    logging.error("a")  # TRY400
    logger.error("a")  # TRY400
    error("a")
```

Checking for a common logger name is generally consistent, but `PGH002` mismatches expectations here.

It might also make sense to look for usages of `logging.getLogger` for a more semantic coverage, even though the vast majority of usages in the wild seem to match our pattern (see also https://grep.app/search?q=logging.getLogger&filter[lang][0]=Python, https://github.com/hummingbot/hummingbot/blob/b3ba248e7b9bab26c010fcb69a53040146084adb/hummingbot/client/config/conf_migration.py#L45, https://github.com/qutebrowser/qutebrowser/blob/4190a470c56ed90d97af89711a30c4603a347858/qutebrowser/utils/log.py#L104)

The main questions i'm trying to answer are, a) should we change the scope of the existing rule (or not) and b) which of the three cases should new logging rules capture?

---

_Label `rule` added by @konstin on 2023-09-18 20:22_

---

_Label `needs-decision` added by @konstin on 2023-09-18 20:22_

---

_Comment by @charliermarsh on 2023-09-19 00:05_

We should be treating `error` as equivalent to `logging.error` -- that seems like a bug in `is_logger_candidate`.

And we should change `PGH002` to match `G010`. Actually, we should probably just remove `PGH002`, and redirect to `G010`?


---

_Referenced in [astral-sh/ruff#7507](../../astral-sh/ruff/pulls/7507.md) on 2023-09-19 03:37_

---

_Comment by @charliermarsh on 2023-09-19 03:53_

> We should be treating `error` as equivalent to `logging.error`

I looked into this a bit and I think it requires some minor modifications to the various rules that call `is_logger_candidate`.

---

_Label `needs-decision` removed by @charliermarsh on 2023-09-19 04:07_

---

_Referenced in [astral-sh/ruff#7410](../../astral-sh/ruff/pulls/7410.md) on 2023-09-19 06:58_

---

_Comment by @qdegraaf on 2023-09-19 15:04_

> I looked into this a bit and I think it requires some minor modifications to the various rules that call `is_logger_candidate`.

Could it not be handled in `is_logger_candidate()` itself?

Am I correct in thinking the issue is that `is_logger_candidate()` assumes the func to be an `Expr::Attribute` whereas a loose `error()` call is an `Expr::Name`? If so, would it be better to add a branch of logic to `is_logger_candidate` for this scenario, seeing as `resolve_call_path()` and `collect_call_path()` already allow for both `Expr::Attribute` and `Expr::Name` values to be passed?

---

_Referenced in [astral-sh/ruff#7521](../../astral-sh/ruff/pulls/7521.md) on 2023-09-19 15:24_

---

_Comment by @charliermarsh on 2023-09-27 00:22_

Closed by https://github.com/astral-sh/ruff/pull/7521.

---

_Closed by @charliermarsh on 2023-09-27 00:22_

---

_Referenced in [astral-sh/ruff#7677](../../astral-sh/ruff/pulls/7677.md) on 2023-09-27 14:08_

---
