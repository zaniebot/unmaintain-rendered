```yaml
number: 9317
title: Respect runtime-required decorators on functions
type: pull_request
state: merged
author: charliermarsh
labels:
  - configuration
assignees: []
merged: true
base: main
head: charlie/runtime-decorator
created_at: 2023-12-29T22:32:14Z
updated_at: 2023-12-30T02:14:54Z
url: https://github.com/astral-sh/ruff/pull/9317
synced_at: 2026-01-12T15:55:28Z
```

# Respect runtime-required decorators on functions

---

_@charliermarsh_

## Summary

This PR modifies the semantics of `runtime-evaluated-decorators` to respect decorators on both classes _and_ functions. Historically, this only respected classes, since the common use-case is (e.g.) `pydantic.BaseModel` -- but functions are equally valid.

Closes https://github.com/astral-sh/ruff/issues/9312.

## Test Plan

`cargo test`


---

_Label `configuration` added by @charliermarsh on 2023-12-29 22:32_

---

_Review requested from @zanieb by @charliermarsh on 2023-12-29 22:33_

---

_Comment by @github-actions[bot] on 2023-12-29 22:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@zanieb approved on 2023-12-29 23:55_

---

_Merged by @charliermarsh on 2023-12-30 02:14_

---

_Closed by @charliermarsh on 2023-12-30 02:14_

---

_Branch deleted on 2023-12-30 02:14_

---
