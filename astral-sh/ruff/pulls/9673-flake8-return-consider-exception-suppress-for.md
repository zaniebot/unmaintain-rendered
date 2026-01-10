```yaml
number: 9673
title: "[flake8-return] Consider exception suppress for unnecessary assignment"
type: pull_request
state: merged
author: mikeleppane
labels: []
assignees: []
merged: true
base: main
head: fix(RET504)/consider-exception-suppress-when-analyzing-unnecessary-assign
created_at: 2024-01-29T12:47:52Z
updated_at: 2024-01-29T17:29:05Z
url: https://github.com/astral-sh/ruff/pull/9673
synced_at: 2026-01-10T22:57:09Z
```

# [flake8-return] Consider exception suppress for unnecessary assignment

---

_Pull request opened by @mikeleppane on 2024-01-29 12:47_

## Summary

This review contains a fix for [RET504](https://docs.astral.sh/ruff/rules/unnecessary-assign/) (unnecessary-assign)

The problem is that Ruff suggests combining a return statement inside contextlib.suppress. Even though it is an unsafe fix it might lead to an invalid code that is not equivalent to the original one.  

See: https://github.com/astral-sh/ruff/issues/5909

## Test Plan

```bash
cargo test
```


---

_Comment by @github-actions[bot] on 2024-01-29 13:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-01-29 17:10_

Thanks! I moved this logic into the visitor, since as-is, I think this would've missed `with` statements that weren't at the end of the function.

---

_Merged by @charliermarsh on 2024-01-29 17:29_

---

_Closed by @charliermarsh on 2024-01-29 17:29_

---
