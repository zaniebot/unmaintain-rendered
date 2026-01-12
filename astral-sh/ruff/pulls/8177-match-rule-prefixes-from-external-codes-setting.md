```yaml
number: 8177
title: "Match rule prefixes from `external` codes setting in `unused-noqa`"
type: pull_request
state: merged
author: zanieb
labels:
  - rule
  - configuration
assignees: []
merged: true
base: main
head: zanie/external-prefix
created_at: 2023-10-24T18:03:48Z
updated_at: 2023-10-24T22:36:55Z
url: https://github.com/astral-sh/ruff/pull/8177
synced_at: 2026-01-12T15:55:25Z
```

# Match rule prefixes from `external` codes setting in `unused-noqa`

---

_@zanieb_

Supersedes https://github.com/astral-sh/ruff/pull/8176
Closes https://github.com/astral-sh/ruff/pull/8174

## Test plan

Old snapshot contains the new / unmatched `V` code
New snapshot contains no `V` prefixed codes

---

_Label `rule` added by @zanieb on 2023-10-24 18:16_

---

_Label `configuration` added by @zanieb on 2023-10-24 18:16_

---

_Comment by @github-actions[bot] on 2023-10-24 18:23_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@charliermarsh reviewed on 2023-10-24 19:57_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/noqa.rs`:134 on 2023-10-24 19:57_

Since most codes _won't_ match an external, I bet it's better to just do this iteration every time and skip the direct check, but obviously can't say for sure.

It's probably even better to just use a vector instead of a hash set, since we're doing this iteration every time, and the set of externals is gonna tend to be quite small.


---

_@charliermarsh approved on 2023-10-24 19:58_

---

_@charliermarsh reviewed on 2023-10-24 19:58_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/noqa.rs`:134 on 2023-10-24 19:58_

(Defer to you as to whether you want to change.)

---

_@zanieb reviewed on 2023-10-24 22:18_

---

_Review comment by @zanieb on `crates/ruff_linter/src/checkers/noqa.rs`:134 on 2023-10-24 22:18_

Okee

---

_Merged by @zanieb on 2023-10-24 22:28_

---

_Closed by @zanieb on 2023-10-24 22:28_

---

_Branch deleted on 2023-10-24 22:28_

---
