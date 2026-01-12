```yaml
number: 6679
title: Avoid panic in unused arguments rule for parameter-free lambda
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/lambda
created_at: 2023-08-18T18:17:46Z
updated_at: 2023-08-18T18:29:32Z
url: https://github.com/astral-sh/ruff/pull/6679
synced_at: 2026-01-12T15:55:22Z
```

# Avoid panic in unused arguments rule for parameter-free lambda

---

_@charliermarsh_

## Summary

This was just a mistake in pattern-matching with no test coverage.

## Test Plan

`cargo test`


---

_@charliermarsh reviewed on 2023-08-18 18:18_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_unused_arguments/rules/unused_arguments.rs`:440 on 2023-08-18 18:18_

Using `parameters: Some(parameters)` meant that we went to the panic branch if the lambda no parameters...

---

_@MichaReiser approved on 2023-08-18 18:20_

---

_Merged by @charliermarsh on 2023-08-18 18:29_

---

_Closed by @charliermarsh on 2023-08-18 18:29_

---

_Branch deleted on 2023-08-18 18:29_

---
