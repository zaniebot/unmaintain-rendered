```yaml
number: 11505
title: "Update parser API references, expose `Program` to linter"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: dhruv/parser-phase-2
head: dhruv/parser-api-refs
created_at: 2024-05-23T04:44:18Z
updated_at: 2024-05-28T04:28:17Z
url: https://github.com/astral-sh/ruff/pull/11505
synced_at: 2026-01-10T21:56:00Z
```

# Update parser API references, expose `Program` to linter

---

_Pull request opened by @dhruvmanila on 2024-05-23 04:44_

## Summary

This PR updates the parser API references per the updated function signatures, especially the return type. Additionally, it exposes the `Program` to the linter by using the new API and also storing it on the `Checker` to be used by the rules.

Most of the changes are in test functions. There are only a handful of usages in the non-test source code.


---

_Label `internal` added by @dhruvmanila on 2024-05-27 17:10_

---

_Marked ready for review by @dhruvmanila on 2024-05-27 17:11_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-05-27 17:11_

---

_Merged by @dhruvmanila on 2024-05-28 04:28_

---

_Closed by @dhruvmanila on 2024-05-28 04:28_

---

_Branch deleted on 2024-05-28 04:28_

---
