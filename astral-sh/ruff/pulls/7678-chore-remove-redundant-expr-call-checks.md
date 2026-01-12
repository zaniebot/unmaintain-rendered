```yaml
number: 7678
title: "chore: remove redundant `Expr::Call` checks"
type: pull_request
state: merged
author: qdegraaf
labels:
  - internal
assignees: []
merged: true
base: main
head: chore/removeredundantcallchecks
created_at: 2023-09-27T14:56:36Z
updated_at: 2023-09-27T15:36:20Z
url: https://github.com/astral-sh/ruff/pull/7678
synced_at: 2026-01-12T02:39:10Z
```

# chore: remove redundant `Expr::Call` checks

---

_Pull request opened by @qdegraaf on 2023-09-27 14:56_

## Summary

As we bind the `ast::ExprCall` in the big `match expr` in `expression.rs` 
```rust
Expr::Call(
    call @ ast::ExprCall {
     ...
```

There is no need for additional `let/if let` checks on `ExprCall` in downstream rules. Found a few older rules which still did this while working on something else. This PR removes the redundant check from these rules.

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2023-09-27 15:13_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@zanieb approved on 2023-09-27 15:14_

Thank you!

---

_Label `internal` added by @zanieb on 2023-09-27 15:14_

---

_@charliermarsh approved on 2023-09-27 15:24_

Thanks, that's great.

---

_Merged by @charliermarsh on 2023-09-27 15:36_

---

_Closed by @charliermarsh on 2023-09-27 15:36_

---
