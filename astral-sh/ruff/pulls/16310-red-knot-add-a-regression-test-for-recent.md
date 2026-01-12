```yaml
number: 16310
title: "[red-knot] Add a regression test for recent improvement to `TypeInferenceBuilder::infer_name_load()`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/moduletype-fallback-test
created_at: 2025-02-21T22:19:15Z
updated_at: 2025-02-21T22:28:43Z
url: https://github.com/astral-sh/ruff/pull/16310
synced_at: 2026-01-12T15:55:54Z
```

# [red-knot] Add a regression test for recent improvement to `TypeInferenceBuilder::infer_name_load()`

---

_@AlexWaygood_

## Summary

This PR adds a regression test for the subtle bug fixed in https://github.com/astral-sh/ruff/pull/16284

## Test Plan

I verified that this test fails if you check out https://github.com/astral-sh/ruff/commit/c2b9fa84f7ffffca0ab173751f5dc2943e8b052d and add the test.


---

_Label `testing` added by @AlexWaygood on 2025-02-21 22:19_

---

_Label `red-knot` added by @AlexWaygood on 2025-02-21 22:19_

---

_Review requested from @carljm by @AlexWaygood on 2025-02-21 22:19_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-02-21 22:19_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-02-21 22:19_

---

_Comment by @AlexWaygood on 2025-02-21 22:20_

It was wonderful how easy it was to write this test. Thanks so much @sharkdp for adding the custom-typeshed feature to mdtest!

---

_@carljm approved on 2025-02-21 22:22_

Very nice!

---

_Merged by @AlexWaygood on 2025-02-21 22:28_

---

_Closed by @AlexWaygood on 2025-02-21 22:28_

---

_Branch deleted on 2025-02-21 22:28_

---
