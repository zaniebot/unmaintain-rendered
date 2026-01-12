```yaml
number: 116
title: Handle E731 in type-annotated assignment
type: pull_request
state: merged
author: cjfuller
labels: []
assignees: []
merged: true
base: main
head: annotated_assign_lambda
created_at: 2022-09-07T00:08:25Z
updated_at: 2022-09-07T00:38:08Z
url: https://github.com/astral-sh/ruff/pull/116
synced_at: 2026-01-12T05:48:45Z
```

# Handle E731 in type-annotated assignment

---

_Pull request opened by @cjfuller on 2022-09-07 00:08_

The DoNotAssignLambda rule was only being handled for assignment with no type annotations. This commit also handles it for annotated assigns and adds a fixture for this case as well.

Tested with: `cargo test` (which failed prior to the addition of the rust changes).

---

_Merged by @charliermarsh on 2022-09-07 00:18_

---

_Closed by @charliermarsh on 2022-09-07 00:18_

---

_Branch deleted on 2022-09-07 00:38_

---
