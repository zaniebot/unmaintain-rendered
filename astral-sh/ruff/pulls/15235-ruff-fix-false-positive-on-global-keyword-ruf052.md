```yaml
number: 15235
title: "[`ruff`] Fix false positive on global keyword (`RUF052`)"
type: pull_request
state: merged
author: Garrett-R
labels:
  - bug
assignees: []
merged: true
base: main
head: garrett/14996/ruff052-fix-global-keyword
created_at: 2025-01-03T05:08:44Z
updated_at: 2025-01-14T16:03:47Z
url: https://github.com/astral-sh/ruff/pull/15235
synced_at: 2026-01-10T20:34:00Z
```

# [`ruff`] Fix false positive on global keyword (`RUF052`)

---

_Pull request opened by @Garrett-R on 2025-01-03 05:08_

## Summary

Fixes #14996.  

BTW, the issue correctly reports a false positive given that [the docs](https://docs.astral.sh/ruff/rules/used-dummy-variable/#why-is-this-bad) say "Only local variables in function scopes are flagged by the rule".

## Test Plan

I added the false positive to the fixture and verified it indeed caused a false positive before my fix was done.


---

_@Garrett-R reviewed on 2025-01-03 05:10_

---

_Review comment by @Garrett-R on `crates/ruff_linter/resources/test/fixtures/ruff/RUF052.py`:1 on 2025-01-03 05:10_

This might be just personal taste, so happy to remove!

But when I saw just `# Correct`, I wasn't sure if that was demarcating a section of code.  I personally find this more clear.

EDIT: should probably use blocker header format (otherwise conflicts with [E266](https://docs.astral.sh/ruff/rules/multiple-leading-hashes-for-block-comment/)).

EDIT 2: updated to block header format, resolving

---

_@Garrett-R reviewed on 2025-01-03 05:15_

---

_Review comment by @Garrett-R on `crates/ruff_linter/resources/test/fixtures/ruff/RUF052.py`:50 on 2025-01-03 05:15_

Note: I inserted the test here into the `# Correct` section, seemed right thing to do.

But given it's not being at the end of this file, it means I got some large diffs in the fixture files.  I reviewed the diffs and there's no "real" changes to them, as expected.

---

_@Garrett-R reviewed on 2025-01-03 05:17_

---

_Review comment by @Garrett-R on `crates/ruff_linter/resources/test/fixtures/ruff/RUF052.py`:42 on 2025-01-03 05:17_

(am I overcommenting? I was just worried that it might not be clear that this is a single "test" spread across 2 functions, but happy to remove)

---

_Comment by @github-actions[bot] on 2025-01-03 05:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @Garrett-R on 2025-01-03 05:20_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/model.rs`:1501 on 2025-01-03 09:03_

I don't think this is the right fix. The binding pushed here is for the variable `_x` in the global scope. But `_x` isn't considered a global-binding in the global scope. It is only when used in a nested scope. For example, the checker explicitly skips over the global handling when in the global scope here

https://github.com/astral-sh/ruff/blob/d4ee6abf4aef3615540a4a84f16edf5e05541622/crates/ruff_linter/src/checkers/ast/mod.rs#L657-L677

But I think you're on the right track. What I find suspicious (and changing it fixes the bug as well) is that `scope` is `self.scope_id` but we add the binding to `self.global_scope_mut`. That's why I think it should actually be `ScopeId::global()` instead. But I'd like a confirmation from @charliermarsh  on this change


---

_@MichaReiser reviewed on 2025-01-03 09:03_

---

_Review requested from @charliermarsh by @MichaReiser on 2025-01-03 09:04_

---

_Comment by @MichaReiser on 2025-01-10 14:51_

@charliermarsh would you mind taking a look at my comment so that we can move forward with this PR?

---

_@charliermarsh reviewed on 2025-01-13 01:54_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:1501 on 2025-01-13 01:54_

(Yes, sorry, will take a look.)

---

_@charliermarsh reviewed on 2025-01-13 02:07_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:1501 on 2025-01-13 02:07_

Hmm... I guess `ScopeId::global()` seems right? We should have decent test coverage here, so if we change that and it passes tests, I'd assume it's correct.

---

_@MichaReiser reviewed on 2025-01-13 07:56_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/model.rs`:1501 on 2025-01-13 07:56_

Thank you. @Garrett-R do you mind changing your PR accordingly? 

---

_@Garrett-R reviewed on 2025-01-14 05:33_

---

_Review comment by @Garrett-R on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__RUF052_RUF052.py.snap`:1 on 2025-01-14 05:33_

Note: I've reviewed the 3 snapshot files.  As expected they have no "real" changes, just line numbers changing.

---

_@Garrett-R reviewed on 2025-01-14 05:35_

---

_Review comment by @Garrett-R on `crates/ruff_python_semantic/src/model.rs`:1501 on 2025-01-14 05:35_

@MichaReiser thank you for that explanation, super helpful! 

Ok, I've updated the PR.

---

_Review requested from @MichaReiser by @Garrett-R on 2025-01-14 05:35_

---

_@MichaReiser approved on 2025-01-14 07:36_

Thanks

---

_Label `bug` added by @MichaReiser on 2025-01-14 07:36_

---

_Merged by @MichaReiser on 2025-01-14 07:36_

---

_Closed by @MichaReiser on 2025-01-14 07:36_

---

_Branch deleted on 2025-01-14 16:03_

---
