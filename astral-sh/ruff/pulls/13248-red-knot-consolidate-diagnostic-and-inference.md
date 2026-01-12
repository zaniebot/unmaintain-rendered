```yaml
number: 13248
title: "[red-knot] consolidate diagnostic and inference tests"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/tests-together
created_at: 2024-09-04T22:05:00Z
updated_at: 2024-09-05T16:15:24Z
url: https://github.com/astral-sh/ruff/pull/13248
synced_at: 2026-01-12T15:55:43Z
```

# [red-knot] consolidate diagnostic and inference tests

---

_@carljm_

Pull the tests from `types.rs` into `infer.rs`.

All of these are integration tests with the same basic form: create a code sample, run type inference or check on it, and make some assertions about types and/or diagnostics. These are the sort of tests we will want to move into a test framework with a low-boilerplate custom textual format. In the meantime, having them together (and more importantly, their helper utilities together) means that it's easy to keep tests for related language features together (iterable tests with other iterable tests, callable tests with other callable tests), without an artificial split based on tests which test diagnostics vs tests which test inference. And it allows a single test to more easily test both diagnostics and inference. (Ultimately in the test framework, they will likely all test diagnostics, just in some cases the diagnostics will come from `reveal_type()`.)

---

_Label `red-knot` added by @carljm on 2024-09-04 22:05_

---

_Review requested from @MichaReiser by @carljm on 2024-09-04 22:05_

---

_Review requested from @AlexWaygood by @carljm on 2024-09-04 22:05_

---

_Comment by @github-actions[bot] on 2024-09-04 22:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-09-05 06:28_

I would be more leaning towards moving the tests in the other direction, considering that `types` is the API but whatever. Let's build a testing framework instead to solve this for good.

---

_@AlexWaygood approved on 2024-09-05 10:31_

---

_Comment by @carljm on 2024-09-05 15:57_

Yeah we could just as well put them in `types.rs`. It's not hard to do, I can push that change. But I agree, hopefully they don't stay in either of these files for too much longer.

---

_Comment by @carljm on 2024-09-05 15:58_

Actually I'm not going to push that change because it will cause too much pain in rebasing my WIP on declared types, where I add a bunch of new tests :) We'll just leave them in `infer.rs` for now until they get moved into a testing framework.

---

_Comment by @MichaReiser on 2024-09-05 16:00_

> Yeah we could just as well put them in `types.rs`. It's not hard to do, I can push that change. But I agree, hopefully they don't stay in either of these files for too much longer.

I'm fine either way. I prefer what ever takes the least of your time (which is what you have now)

---

_Merged by @carljm on 2024-09-05 16:15_

---

_Closed by @carljm on 2024-09-05 16:15_

---

_Branch deleted on 2024-09-05 16:15_

---
