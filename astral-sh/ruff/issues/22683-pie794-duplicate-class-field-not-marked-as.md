```yaml
number: 22683
title: PIE794 Duplicate class field not marked as duplicate
type: issue
state: open
author: bhirsz
labels: []
assignees: []
created_at: 2026-01-18T16:16:26Z
updated_at: 2026-01-18T16:16:26Z
url: https://github.com/astral-sh/ruff/issues/22683
synced_at: 2026-01-18T17:26:19Z
```

# PIE794 Duplicate class field not marked as duplicate

---

_@bhirsz_

### Summary

Playground link: https://play.ruff.rs/e8e979ae-a4d2-49a1-b883-7242fc0a2a6c

```
from dataclasses import dataclass


@dataclass
class TextEdit:
    start_line: int
    start_line: int
```

I have Python dataclass with duplicated class field. It didn't trigger any ruff rule. The closes rule I have found is PIE794 which reports duplicate class fields (with values assigned).

### Version

ruff v0.14.13

---
