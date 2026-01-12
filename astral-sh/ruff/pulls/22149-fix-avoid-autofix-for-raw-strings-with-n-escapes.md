```yaml
number: 22149
title: "fix: avoid autofix for raw strings with \\N escapes"
type: pull_request
state: open
author: denyszhak
labels: []
assignees: []
base: main
head: fix/up032-raw-n-escape
created_at: 2025-12-22T22:59:00Z
updated_at: 2026-01-03T23:49:45Z
url: https://github.com/astral-sh/ruff/pull/22149
synced_at: 2026-01-12T15:57:43Z
```

# fix: avoid autofix for raw strings with \N escapes

---

_@denyszhak_

**Summary:**

Fixes UP032 autofix incorrectly converting raw strings with `\N{...}` to f-strings, which changes semantics and causes runtime errors.

Fixes #22060 

## Test Plan

- Added test case for raw strings with \N{...}
- Regular strings with \N{...} still autofix correctly
- All 119 pyupgrade tests pass

---

_Comment by @astral-sh-bot[bot] on 2025-12-22 23:12_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Comment by @dscorbett on 2025-12-23 14:23_

This PR suppresses the fix when a raw string contains `\N{`. That is suboptimal because there is no need to suppress the fix. For example, `r"\N{angle}AOB = {angle}°".format(angle=180)` can safely be fixed to `rf"\N{180}AOB = {180}°"`. Instead, a better solution is to suppress for raw strings the `pending_escape` logic introduced in #21901.

---

_Converted to draft by @denyszhak on 2025-12-25 12:08_

---

_Marked ready for review by @denyszhak on 2026-01-03 23:49_

---

_Comment by @denyszhak on 2026-01-03 23:49_

> This PR suppresses the fix when a raw string contains `\N{`. That is suboptimal because there is no need to suppress the fix. For example, `r"\N{angle}AOB = {angle}°".format(angle=180)` can safely be fixed to `rf"\N{180}AOB = {180}°"`. Instead, a better solution is to suppress for raw strings the `pending_escape` logic introduced in #21901.

@dscorbett @ntBre does it make sense now?

---
