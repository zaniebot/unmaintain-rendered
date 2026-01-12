```yaml
number: 17648
title: "[`pyupgrade`] Add spaces between tokens as necessary to avoid syntax errors in `UP018` autofix"
type: pull_request
state: merged
author: LaBatata101
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-UP018
created_at: 2025-04-26T21:03:41Z
updated_at: 2025-05-07T07:34:08Z
url: https://github.com/astral-sh/ruff/pull/17648
synced_at: 2026-01-12T15:56:03Z
```

# [`pyupgrade`] Add spaces between tokens as necessary to avoid syntax errors in `UP018` autofix

---

_@LaBatata101_

## Summary

Is it better to add another edit in the fix to add the space, or this is fine?
https://github.com/LaBatata101/ruff/blob/032bd2edcb7a3576ac0a69fd193a675639b5ea0e/crates/ruff_linter/src/rules/pyupgrade/rules/native_literals.rs#L284-L286

Fixes #17606

## Test Plan
Snapshot tests.

---

_Comment by @github-actions[bot] on 2025-04-26 21:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @AlexWaygood on 2025-04-27 11:41_

---

_Label `fixes` added by @AlexWaygood on 2025-04-27 11:41_

---

_Renamed from "[`pyupgrade`] Add spaces between tokens as necessary to avoid syntax errors in `UP018` fix" to "[`pyupgrade`] Add spaces between tokens as necessary to avoid syntax errors in `UP018` autofix" by @LaBatata101 on 2025-04-27 20:51_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/native_literals.rs`:254 on 2025-05-03 14:08_

Would it be possible to use `after` or `in_range` here over implementing the binary search manually

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/native_literals.rs`:256 on 2025-05-03 14:17_

```suggestion
                let token = &tokens[token_idx];
```

---

_@MichaReiser reviewed on 2025-05-03 14:19_

I think what you did is fine. I'm somewhat inclined to instead look at the `parent_expr` but I can see that this might be harder (simply because we already use it in the check)

---

_@LaBatata101 reviewed on 2025-05-03 19:39_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/pyupgrade/rules/native_literals.rs`:254 on 2025-05-03 19:39_

Yeah, we can use `after` I've made a commit using `after` instead of the binary search.

---

_Review requested from @MichaReiser by @LaBatata101 on 2025-05-03 19:39_

---

_@MichaReiser approved on 2025-05-07 07:34_

---

_Merged by @MichaReiser on 2025-05-07 07:34_

---

_Closed by @MichaReiser on 2025-05-07 07:34_

---
