```yaml
number: 10248
title: Make rule PT012 example clearer
type: pull_request
state: merged
author: eerovaher
labels:
  - documentation
assignees: []
merged: true
base: main
head: PT012-example
created_at: 2024-03-06T15:25:31Z
updated_at: 2024-03-06T15:53:21Z
url: https://github.com/astral-sh/ruff/pull/10248
synced_at: 2026-01-12T15:55:31Z
```

# Make rule PT012 example clearer

---

_@eerovaher_

_No description provided._

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-03-06 15:27_

---

_Label `documentation` added by @MichaReiser on 2024-03-06 15:27_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/raises.rs`:31 on 2024-03-06 15:33_

I think this proposed change defeats the point of the example.

In this example, the person writing the test wants to assert that `func_to_test` raises `MyError`. But the test they're writing has a bug in it -- the `setup()` function raises `MyError`, which means that the test passes even though the `pytest.raises()` block is exited before we even get to the function they wanted to test. The comment here is trying to point to the line that that should be moved out of the `pytest.raises()` block in order to avoid this antipattern

---

_@AlexWaygood requested changes on 2024-03-06 15:33_

---

_Comment by @github-actions[bot] on 2024-03-06 15:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2024-03-06 15:42_

Thanks!

---

_@eerovaher reviewed on 2024-03-06 15:43_

---

_Review comment by @eerovaher on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/raises.rs`:31 on 2024-03-06 15:43_

From the example solution I got the impression that `setup()` cannot raise `MyError` because if it does then simply moving it out from the `with`-block is not the correct thing to do. But it was simple enough to address your feedback by making the comment more explicit.

---

_@AlexWaygood reviewed on 2024-03-06 15:45_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/raises.rs`:31 on 2024-03-06 15:45_

Yeah, I agree that it wasn't particularly clear as it was. I like the revised PR, thanks!

---

_Merged by @AlexWaygood on 2024-03-06 15:47_

---

_Closed by @AlexWaygood on 2024-03-06 15:47_

---

_Branch deleted on 2024-03-06 15:51_

---
