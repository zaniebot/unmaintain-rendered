```yaml
number: 13656
title: "[`flake8-bugbear`] Tweak `B905` message to not suggest setting parameter `strict=` to `False`"
type: pull_request
state: merged
author: qdegraaf
labels:
  - rule
assignees: []
merged: true
base: main
head: feat/b905message
created_at: 2024-10-07T10:04:53Z
updated_at: 2024-10-07T12:05:30Z
url: https://github.com/astral-sh/ruff/pull/13656
synced_at: 2026-01-12T15:55:45Z
```

# [`flake8-bugbear`] Tweak `B905` message to not suggest setting parameter `strict=` to `False`

---

_@qdegraaf_

## Summary

Tweaks the message of `B905` to not contradict the [B905 docs](https://docs.astral.sh/ruff/rules/zip-without-explicit-strict/#zip-without-explicit-strict-b905), leaves the autofix alone so as to avoid changing semantics or code behaviour. See also: https://github.com/astral-sh/ruff/issues/13581#issuecomment-2384800857 

## Test Plan

`cargo test`

## Issue link
Closes: https://github.com/astral-sh/ruff/issues/13581 


---

_Comment by @github-actions[bot] on 2024-10-07 10:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2024-10-07 11:52_

Thanks!

---

_Label `rule` added by @AlexWaygood on 2024-10-07 11:52_

---

_Merged by @AlexWaygood on 2024-10-07 11:56_

---

_Closed by @AlexWaygood on 2024-10-07 11:56_

---
