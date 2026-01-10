```yaml
number: 20573
title: "[ty] Add more tests for subtyping/assignability between two protocol types"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/iter-union-tests
created_at: 2025-09-25T13:40:01Z
updated_at: 2025-09-26T11:07:59Z
url: https://github.com/astral-sh/ruff/pull/20573
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Add more tests for subtyping/assignability between two protocol types

---

_Pull request opened by @AlexWaygood on 2025-09-25 13:40_

## Summary

Currently we have lots of tests for subtyping and assignability where one type is nominal and the other type is a protocol type. However, we have few tests for subtyping/assignability between two types where both types are protocols, which is a different code path. This PR adds that missing test coverage.

Specifically, this PR:
- Adds missing tests for subtyping/assignability between two protocols with attribute members
- Adds missing tests for subtyping/assignability between two protocols with `@property` members
- Adds missing tests for subtyping/assignability between two protocols with method members
- Adds missing tests for subtyping/assignability (in both directions) between two protocols where one has a method member and the other has a `@property` member
- Adds missing tests for subtyping/assignability (in both directions) between two protocols where one has a method member and the other has an attribute member
- Adds missing tests for subtyping/assignability (in both directions) between two protocols where one has an attribute member and the other has a `@property` member
- Fixes some pre-existing bugs in our tests for protocols with `@property` members
- Adds regression tests for https://github.com/astral-sh/ty/issues/1089


---

_Review requested from @carljm by @AlexWaygood on 2025-09-25 13:40_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-09-25 13:40_

---

_Review requested from @dcreager by @AlexWaygood on 2025-09-25 13:40_

---

_Label `testing` added by @AlexWaygood on 2025-09-25 13:40_

---

_Label `ty` added by @AlexWaygood on 2025-09-25 13:40_

---

_@carljm approved on 2025-09-25 23:17_

---

_Merged by @AlexWaygood on 2025-09-26 11:07_

---

_Closed by @AlexWaygood on 2025-09-26 11:07_

---

_Branch deleted on 2025-09-26 11:07_

---
