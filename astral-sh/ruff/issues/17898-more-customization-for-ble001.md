```yaml
number: 17898
title: More customization for BLE001
type: issue
state: open
author: sassanh
labels:
  - rule
assignees: []
created_at: 2025-05-06T21:53:28Z
updated_at: 2025-05-07T06:31:57Z
url: https://github.com/astral-sh/ruff/issues/17898
synced_at: 2026-01-12T15:54:56Z
```

# More customization for BLE001

---

_@sassanh_

### Summary

Currently if there is a `logger.exception()` inside the `catch Exception:` block, it will pass BLE001, the thing is one might have their own exception reporter function that possibly internally calls `logger.exception()` or log the exception in a totally different way.

It would be nice if we could somehow customize the phrases that would make BLE001 pass if present in `catch Exception:` block.

---

_Comment by @MichaReiser on 2025-05-07 06:31_

Related to https://github.com/astral-sh/ruff/issues/15473

---

_Label `rule` added by @MichaReiser on 2025-05-07 06:31_

---
