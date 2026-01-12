```yaml
number: 10288
title: Improve various assignment target error
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: dhruv/parser
head: dhruv/assign-target-error
created_at: 2024-03-08T02:51:02Z
updated_at: 2024-03-08T17:54:10Z
url: https://github.com/astral-sh/ruff/pull/10288
synced_at: 2026-01-12T15:55:31Z
```

# Improve various assignment target error

---

_@dhruvmanila_

## Summary

This PR improves error related things around assignment nodes, mainly the following:
1. Rename parse error variant:
	a. `AssignmentError` -> `InvalidAssignmentTarget`
	b. `NamedAssignmentError` -> `InvalidNamedAssignmentTarget`
	c. `AugAssignmentError` -> `InvalidAugmnetedAssignmentTarget`
2. Add `InvalidDeleteTarget` for invalid `del` targets
	a. Add helper function to check if it's a valid delete target similar to other target check functions.
4. Fix: named assignment target can only be a `Name` node

## Test Plan

Various test cases locally. As mentioned in my previous PR, I want to keep the testing part separate.


---

_Label `parser` added by @dhruvmanila on 2024-03-08 02:51_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-03-08 02:51_

---

_Merged by @dhruvmanila on 2024-03-08 17:54_

---

_Closed by @dhruvmanila on 2024-03-08 17:54_

---

_Branch deleted on 2024-03-08 17:54_

---
