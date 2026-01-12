```yaml
number: 18854
title: "[`RUF`] [preview] `RUF037` could support empty strings"
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - rule
assignees: []
created_at: 2025-06-21T20:22:19Z
updated_at: 2025-06-24T06:26:29Z
url: https://github.com/astral-sh/ruff/issues/18854
synced_at: 2026-01-12T15:54:56Z
```

# [`RUF`] [preview] `RUF037` could support empty strings

---

_@MeGaGiGaGon_

### Summary

[unnecessary-empty-iterable-within-deque-call (RUF037)](https://docs.astral.sh/ruff/rules/unnecessary-empty-iterable-within-deque-call/#unnecessary-empty-iterable-within-deque-call-ruf037) currently does not support empty strings. Based on my github searches, this is an uncommon but present pattern, so could be a potential improvement.
[playground](https://play.ruff.rs/6af8d1b3-1640-4eff-b320-e33e2046b070)
```py
from collections import deque

queue = deque("")  # No error
queue = deque(b"")  # No error
```
Search for `"deque('')"`, 34 results [link](https://github.com/search?q=%22deque%28%27%27%29%22&type=code)
Search for `"deque(\"\")"`, 12 results [link](https://github.com/search?q=%22deque%28%5C%22%5C%22%29%22&type=code)
Search for `"deque(b\"\")"`, 2 results [link](https://github.com/search?q=%22deque%28b%5C%22%5C%22%29%22&type=code)
Search for `"deque(b'')"`, 1 result [link](https://github.com/search?q=%22deque%28b%27%27%29%22&type=code)

---

_Label `rule` added by @ntBre on 2025-06-22 03:25_

---

_Renamed from "[`Ruff-specific rules`] [preview] `RUF037` could support empty strings" to "[`RUF`] [preview] `RUF037` could support empty strings" by @MichaReiser on 2025-06-23 06:51_

---

_Closed by @MichaReiser on 2025-06-24 06:26_

---
