```yaml
number: 18253
title: Rule BLE001 blind-except triggered even for logger.error
type: issue
state: closed
author: wereii
labels: []
assignees: []
created_at: 2025-05-22T12:38:10Z
updated_at: 2025-05-22T12:40:10Z
url: https://github.com/astral-sh/ruff/issues/18253
synced_at: 2026-01-12T15:54:56Z
```

# Rule BLE001 blind-except triggered even for logger.error

---

_@wereii_

### Summary

Example file:

```py
import logging

logger = logging.getLogger(__name__)


try:
    pass
except Exception as exc:
    logger.error("...", exc_info=exc)
```

tested with `ruff check --select=BLE001`

https://play.ruff.rs/240994a2-9c15-4996-9129-a3bc53c5b605


### Version

0.11.10

---

_Comment by @wereii on 2025-05-22 12:40_

Ah, duplicate of https://github.com/astral-sh/ruff/issues/15473

---

_Closed by @wereii on 2025-05-22 12:40_

---
