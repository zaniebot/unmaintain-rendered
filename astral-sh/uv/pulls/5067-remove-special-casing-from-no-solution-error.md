```yaml
number: 5067
title: Remove special casing from no solution error
type: pull_request
state: merged
author: konstin
labels:
  - internal
  - error messages
assignees: []
merged: true
base: main
head: konsti/simplify-no-solution-error
created_at: 2024-07-15T10:41:55Z
updated_at: 2024-07-15T15:43:37Z
url: https://github.com/astral-sh/uv/pull/5067
synced_at: 2026-01-12T16:06:37Z
```

# Remove special casing from no solution error

---

_@konstin_

The only pubgrub error that can occur is a `NoSolutionError`, and the only place it can occur is `unit_propagation`, all other variants if `PubGrubError` are unreachable. By changing the return type on pubgrub's side (https://github.com/astral-sh/pubgrub/pull/28), we can remove the pattern matching and the `unreachable!()` asserts on `PubGrubError`.

Our pubgrub error wrapper used to have a two phased initialization, first mostly stubs in `solve[_tracked]()` and then adding the actual context in `resolve()`. When constructing the error in `solve` we already have all this context, so we can unify this to a regular constructor and remove the special casing in `resolve()` and `hints()`.


---

_Label `internal` added by @konstin on 2024-07-15 10:41_

---

_Label `error messages` added by @konstin on 2024-07-15 10:41_

---

_Review requested from @zanieb by @konstin on 2024-07-15 10:41_

---

_@konstin reviewed on 2024-07-15 10:42_

---

_Review comment by @konstin on `crates/uv-resolver/src/error.rs`:155 on 2024-07-15 10:42_

This is the same `collapse_proxies` logic as a method

---

_@konstin reviewed on 2024-07-15 10:43_

---

_Review comment by @konstin on `crates/uv-resolver/src/pubgrub/report.rs`:1 on 2024-07-15 10:43_

Same logic, but de-indented since we removed some `Option` wrappers

---

_@konstin reviewed on 2024-07-15 10:44_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:251 on 2024-07-15 10:44_

We have to pass this manually since it's the only thing the solver otherwise doesn't know about when constructing the error.

---

_@charliermarsh approved on 2024-07-15 13:36_

---

_Merged by @konstin on 2024-07-15 15:43_

---

_Closed by @konstin on 2024-07-15 15:43_

---

_Branch deleted on 2024-07-15 15:43_

---
