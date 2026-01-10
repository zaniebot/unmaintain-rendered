```yaml
number: 12013
title: Fix missing related settings header
type: pull_request
state: merged
author: MichaReiser
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix-related-settings
created_at: 2024-06-24T10:21:10Z
updated_at: 2024-06-24T10:32:45Z
url: https://github.com/astral-sh/ruff/pull/12013
synced_at: 2026-01-10T21:56:00Z
```

# Fix missing related settings header

---

_Pull request opened by @MichaReiser on 2024-06-24 10:21_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/12012

This must have broken when I introduced the new top-level `lint` section. The issue was that
we probed for the `<linter>` config option instead of `lint.<linter>`. 

## Test Plan

![image](https://github.com/astral-sh/ruff/assets/1203881/143baeb3-1769-4aed-81dd-63e549012aae)

I verified that the link jumps to the right section


---

_Label `documentation` added by @MichaReiser on 2024-06-24 10:21_

---

_@dhruvmanila approved on 2024-06-24 10:22_

Thank you!

---

_Merged by @MichaReiser on 2024-06-24 10:29_

---

_Closed by @MichaReiser on 2024-06-24 10:29_

---

_Branch deleted on 2024-06-24 10:29_

---

_Comment by @github-actions[bot] on 2024-06-24 10:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
