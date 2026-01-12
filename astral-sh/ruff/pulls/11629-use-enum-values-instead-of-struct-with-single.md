```yaml
number: 11629
title: Use enum values instead of struct with single field
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: dhruv/parser-phase-2
head: dhruv/one-field-struct
created_at: 2024-05-31T04:59:38Z
updated_at: 2024-05-31T04:59:49Z
url: https://github.com/astral-sh/ruff/pull/11629
synced_at: 2026-01-12T15:55:38Z
```

# Use enum values instead of struct with single field

---

_@dhruvmanila_

## Summary

This PR is a follow-up to #11475 to use enum values where there's struct with a single field. The main motivation to use struct was to keep the snapshot update to a minimum which allows me to validate it. Now that the validation is done, we can revert it back.

## Test Plan

Update the snapshots.


---

_Label `internal` added by @dhruvmanila on 2024-05-31 04:59_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-05-31 04:59_

---

_Merged by @dhruvmanila on 2024-05-31 04:59_

---

_Closed by @dhruvmanila on 2024-05-31 04:59_

---

_Branch deleted on 2024-05-31 04:59_

---
