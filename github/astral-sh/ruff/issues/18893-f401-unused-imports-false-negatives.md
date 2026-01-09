---
number: 18893
title: F401 (unused imports) False Negatives
type: issue
state: closed
author: Luux
labels: []
assignees: []
created_at: 2025-06-23T11:26:20Z
updated_at: 2025-06-23T11:58:21Z
url: https://github.com/astral-sh/ruff/issues/18893
synced_at: 2026-01-07T13:12:16-06:00
---

# F401 (unused imports) False Negatives

---

_Issue opened by @Luux on 2025-06-23 11:26_

### Summary

Heyey,

just realized that ruff does not detect certain unused imports. Here's a minimal example:

```
"""Minimal reproduction of ruff unused import bug."""

import langchain_core.language_models
import langchain_core.output_parsers  # unused import


class Prompter:
    def __init__(self, llm: langchain_core.language_models.BaseChatModel):
        self._llm = llm

```

The import is clearly unused in the code, yet ruff does not complain:
```
> ruff check --select F401 minimal_bug_reproduction.py
All checks passed!
```

I use the latest version:
```
ruff --version               
ruff 0.12.0
```

### Version

0.12.0

---

_Comment by @ntBre on 2025-06-23 11:58_

Thanks for the report! I think this is a duplicate of a very old issue: #60.

---

_Closed by @ntBre on 2025-06-23 11:58_

---

_Referenced in [astral-sh/ruff#18985](../../astral-sh/ruff/issues/18985.md) on 2025-06-27 16:00_

---
