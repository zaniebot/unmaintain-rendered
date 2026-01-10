```yaml
number: 14625
title: Avoid invalid syntax for format-spec with quotes for all Python versions
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - formatter
  - preview
assignees: []
merged: true
base: main
head: dhruv/f-string-format-spec-pre-312
created_at: 2024-11-27T05:50:20Z
updated_at: 2024-11-27T07:49:36Z
url: https://github.com/astral-sh/ruff/pull/14625
synced_at: 2026-01-10T20:50:57Z
```

# Avoid invalid syntax for format-spec with quotes for all Python versions

---

_Pull request opened by @dhruvmanila on 2024-11-27 05:50_

## Summary

fixes: #14608

The logic that was only applied for 3.12+ target version needs to be applied for other versions as well.

## Test Plan

I've moved the existing test cases for 3.12 only to `f_string.py` so that it's tested against the default target version.

I think we should probably enabled testing for two target version (pre 3.12 and 3.12) but it won't highlight any issue because the parser doesn't consider this. Maybe we should enable this once we have target version specific syntax errors in place (https://github.com/astral-sh/ruff/issues/6591).


---

_Label `bug` added by @dhruvmanila on 2024-11-27 05:50_

---

_Label `formatter` added by @dhruvmanila on 2024-11-27 05:50_

---

_Label `preview` added by @dhruvmanila on 2024-11-27 05:50_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-11-27 05:50_

---

_Comment by @github-actions[bot] on 2024-11-27 05:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@MichaReiser approved on 2024-11-27 07:43_

---

_Comment by @MichaReiser on 2024-11-27 07:43_

Thank you

---

_Merged by @dhruvmanila on 2024-11-27 07:49_

---

_Closed by @dhruvmanila on 2024-11-27 07:49_

---

_Branch deleted on 2024-11-27 07:49_

---
