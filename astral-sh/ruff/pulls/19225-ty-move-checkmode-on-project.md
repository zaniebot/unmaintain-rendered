```yaml
number: 19225
title: "[ty] Move `CheckMode` on `Project`"
type: pull_request
state: closed
author: dhruvmanila
labels:
  - internal
  - ty
assignees: []
draft: true
base: dhruv/track-open-files-in-server
head: dhruv/move-check-mode
created_at: 2025-07-09T06:16:16Z
updated_at: 2025-07-10T14:41:15Z
url: https://github.com/astral-sh/ruff/pull/19225
synced_at: 2026-01-12T15:56:34Z
```

# [ty] Move `CheckMode` on `Project`

---

_@dhruvmanila_

## Summary

Follow-up from #19181 and as discussed in that PR, this PR moves the `CheckMode` to be part of the `Project` instead of a parameter.

This is used in the follow-up PR (#19182) that makes sure that checking a file / project and constructing the diagnostic builder considers the check mode instead of just checking whether the file is open or not.

## Test Plan

Refer #19182 

---

_Label `internal` added by @dhruvmanila on 2025-07-09 06:16_

---

_Label `ty` added by @dhruvmanila on 2025-07-09 06:16_

---

_Comment by @github-actions[bot] on 2025-07-09 06:20_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Closed by @dhruvmanila on 2025-07-10 14:36_

---

_Branch deleted on 2025-07-10 14:41_

---
