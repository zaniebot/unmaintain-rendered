```yaml
number: 1300
title: "Feature: completion for long'ish control flow statements"
type: issue
state: closed
author: twoertwein
labels:
  - completions
assignees: []
created_at: 2025-10-02T17:42:29Z
updated_at: 2025-10-02T17:52:44Z
url: https://github.com/astral-sh/ty/issues/1300
synced_at: 2026-01-12T15:54:24Z
```

# Feature: completion for long'ish control flow statements

---

_@twoertwein_

### Summary

It would be nice to auto-complete control flow statements like `continue`. Maybe also other statements that are at least four characters long: `return`, `match`, `case`, `break`, ... (not much benefit in auto-completing `if` or `for`)

### Version

_No response_

---

_Label `completions` added by @AlexWaygood on 2025-10-02 17:51_

---

_Comment by @AlexWaygood on 2025-10-02 17:52_

Thanks! I think this is covered by the "Offer contextual language keywords in completions (e.g., `def` and `match`)" bullet in #86

---

_Closed by @AlexWaygood on 2025-10-02 17:52_

---
