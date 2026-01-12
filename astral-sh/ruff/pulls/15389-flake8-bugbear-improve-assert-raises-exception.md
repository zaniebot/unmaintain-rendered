```yaml
number: 15389
title: "[`flake8-bugbear`] Improve assert-raises-exception (B017) message"
type: pull_request
state: merged
author: tjkuson
labels:
  - rule
assignees: []
merged: true
base: main
head: evil-begone
created_at: 2025-01-10T01:37:47Z
updated_at: 2025-01-10T10:25:04Z
url: https://github.com/astral-sh/ruff/pull/15389
synced_at: 2026-01-12T15:55:51Z
```

# [`flake8-bugbear`] Improve assert-raises-exception (B017) message

---

_@tjkuson_

## Summary

Changes the [assert-raises-exception (`B017`)](https://docs.astral.sh/ruff/rules/assert-raises-exception/#assert-raises-exception-b017) message to be more helpful. In doing so, the message is shorter and the implementation simpler as the diagnostic no longer needs to print the assertion.

```diff
- B017.py:45:10: B017 `pytest.raises(Exception)` should be considered evil
+ B017.py:45:10: B017 Do not assert blind exception: `Exception`
```

This assertion message is a corollary of [blind-except (`BLE001`)](https://docs.astral.sh/ruff/rules/blind-except/#blind-except-ble001).

```
Do not catch blind exception: {name}
```

## Test Plan

`cargo nextest run`


---

_Comment by @github-actions[bot] on 2025-01-10 01:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` added by @MichaReiser on 2025-01-10 07:47_

---

_Comment by @MichaReiser on 2025-01-10 07:47_

Thanks

---

_Merged by @MichaReiser on 2025-01-10 07:48_

---

_Closed by @MichaReiser on 2025-01-10 07:48_

---

_Branch deleted on 2025-01-10 10:25_

---
