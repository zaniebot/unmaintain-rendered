```yaml
number: 22613
title: Combine suppression code diagnostics
type: pull_request
state: open
author: amyreese
labels:
  - rule
  - suppression
assignees: []
base: main
head: amy/suppression-grouping
created_at: 2026-01-16T00:34:42Z
updated_at: 2026-01-16T00:50:33Z
url: https://github.com/astral-sh/ruff/pull/22613
synced_at: 2026-01-16T01:04:07Z
```

# Combine suppression code diagnostics

---

_@amyreese_

# Summary

- Report a single combined UnusedNOQA diagnostic when any range suppression
  has one or more unused, disabled, or duplicate lint codes.
- Report a single combined InvalidRuleCode diagnostic for any range suppression
  with one or more invalid lint codes.

# Test Plan

Updated fixtures and snapshots

Fixes #22429, #21873


---

_Comment by @astral-sh-bot[bot] on 2026-01-16 00:44_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Label `rule` added by @amyreese on 2026-01-16 00:50_

---

_Label `suppression` added by @amyreese on 2026-01-16 00:50_

---

_Review requested from @MichaReiser by @amyreese on 2026-01-16 00:50_

---

_Review requested from @ntBre by @amyreese on 2026-01-16 00:50_

---
