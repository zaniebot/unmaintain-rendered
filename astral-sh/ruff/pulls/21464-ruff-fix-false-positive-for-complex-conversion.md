```yaml
number: 21464
title: "[`ruff`] Fix false positive for complex conversion specifiers in `logging-eager-conversion` (`RUF065`)"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: fix-21458
created_at: 2025-11-14T22:47:20Z
updated_at: 2025-11-19T08:38:33Z
url: https://github.com/astral-sh/ruff/pull/21464
synced_at: 2026-01-12T15:57:25Z
```

# [`ruff`] Fix false positive for complex conversion specifiers in `logging-eager-conversion` (`RUF065`)

---

_@danparizher_

## Summary

Fixes a false positive in `RUF065` when `oct()` or `hex()` are used with conversion specifiers that have complex flags or precision. 

Fixes #21458.

## Problem

The rule incorrectly flagged `oct()` and `hex()` calls as unnecessary when conversion specifiers had flags (`0`, ` `, `+`) or precision, which change behavior between `%s` and `%#o`/`%#x`.

## Approach

Added `has_complex_conversion_specifier()` helper to detect complex specifiers (zero-pad with width, blank sign, sign char, or precision) and updated `oct()` and `hex()` checks to skip reporting when the specifier is complex.

## Test Plan

Added test cases to `RUF065_0.py` covering `%06s`, `% s`, `%+s`, and `%.3s` with `oct()` and `hex()`.

---

_Comment by @astral-sh-bot[bot] on 2025-11-14 22:55_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Label `fixes` added by @amyreese on 2025-11-18 17:22_

---

_Review requested from @amyreese by @ntBre on 2025-11-18 21:42_

---

_Comment by @amyreese on 2025-11-18 21:44_

Pushed some changes to reduce verbosity (and duplication) of comments, and to condense the logic in `has_complex_conversion_specifier`. Also moved the "OK" cases into `RUF065_1.py` alongside similar cases, and included a link to the github issue.

---

_Review requested from @ntBre by @amyreese on 2025-11-18 21:45_

---

_@amyreese approved on 2025-11-18 21:45_

---

_Label `fixes` removed by @MichaReiser on 2025-11-19 08:38_

---

_Label `bug` added by @MichaReiser on 2025-11-19 08:38_

---

_Label `rule` added by @MichaReiser on 2025-11-19 08:38_

---

_Merged by @MichaReiser on 2025-11-19 08:38_

---

_Closed by @MichaReiser on 2025-11-19 08:38_

---
