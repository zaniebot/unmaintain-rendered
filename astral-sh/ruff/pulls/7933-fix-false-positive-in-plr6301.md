```yaml
number: 7933
title: "Fix false positive in `PLR6301`"
type: pull_request
state: merged
author: LaBatata101
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-PLR6301-false-positive
created_at: 2023-10-13T02:16:57Z
updated_at: 2023-10-14T19:46:45Z
url: https://github.com/astral-sh/ruff/pull/7933
synced_at: 2026-01-12T02:32:41Z
```

# Fix false positive in `PLR6301`

---

_Pull request opened by @LaBatata101 on 2023-10-13 02:16_

## Summary

Don't report a diagnostic if the method contains a `super()` call.

Closes #6961

## Test Plan

`cargo test`


---

_@LaBatata101 reviewed on 2023-10-13 02:23_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/pylint/rules/no_self_use.rs`:151 on 2023-10-13 02:23_

Ideally, it would be better to use the function scope to check if the method contains a `super()` call in the body, but the function scope only gives me access to its parameters. So when using `scope.get("super")` it would only return `None`.

---

_Comment by @github-actions[bot] on 2023-10-13 02:33_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@charliermarsh reviewed on 2023-10-13 19:50_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/no_self_use.rs`:151 on 2023-10-13 19:50_

It turns out that `super` is actually a global builtin in Python, so the usages we need are on the _global_ scope. I tweaked the logic to use the technique you suggested above. I guess the downside is that we can't ensure that the `super` usage was on a `super()` call, but perhaps that's fine. (We don't store a `NodeId` on `Reference`, though we could in the future.)

---

_Label `bug` added by @charliermarsh on 2023-10-13 19:50_

---

_@charliermarsh reviewed on 2023-10-13 19:51_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/no_self_use.rs`:129 on 2023-10-13 19:51_

@LaBatata101 - What do you think of this?

---

_@LaBatata101 reviewed on 2023-10-13 23:55_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/pylint/rules/no_self_use.rs`:129 on 2023-10-13 23:55_

LGTM. Didn't know about the `binding.references()` function, will be very helpful when implementing other rules in the future.

---

_Review requested from @charliermarsh by @LaBatata101 on 2023-10-13 23:55_

---

_Merged by @charliermarsh on 2023-10-14 18:55_

---

_Closed by @charliermarsh on 2023-10-14 18:55_

---

_Branch deleted on 2023-10-14 19:46_

---
