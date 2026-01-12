```yaml
number: 7686
title: "Avoid unnecessary comments check in `maybe_parenthesize_expression`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/ref
created_at: 2023-09-28T00:30:06Z
updated_at: 2023-09-28T17:42:13Z
url: https://github.com/astral-sh/ruff/pull/7686
synced_at: 2026-01-12T02:39:10Z
```

# Avoid unnecessary comments check in `maybe_parenthesize_expression`

---

_Pull request opened by @charliermarsh on 2023-09-28 00:30_

## Summary

No-op refactor, but we can evaluate early if the first part of `preserve_parentheses || has_comments` is `true`, and thus avoid looking up the node comments.

## Test Plan

`cargo test`


---

_Label `formatter` added by @charliermarsh on 2023-09-28 00:30_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-09-28 00:44_

---

_@MichaReiser approved on 2023-09-28 06:44_

Nice. 

This PR does something similar by having a `needs_parentheses` method that returns the "resolved" `OptionalParentheses`

https://github.com/astral-sh/ruff/blob/d1a76d00112c3252ac3a17b7ecdc0d2bc8c2e8b0/crates/ruff_python_formatter/src/expression/mod.rs#L183-L219

---

_Merged by @charliermarsh on 2023-09-28 17:42_

---

_Closed by @charliermarsh on 2023-09-28 17:42_

---

_Branch deleted on 2023-09-28 17:42_

---
