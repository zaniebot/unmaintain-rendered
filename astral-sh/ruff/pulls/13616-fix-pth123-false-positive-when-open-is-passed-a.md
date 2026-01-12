```yaml
number: 13616
title: "Fix `PTH123` false positive when `open` is passed a file descriptor"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/pathlib-open-fd
created_at: 2024-10-03T17:53:43Z
updated_at: 2024-10-04T13:48:48Z
url: https://github.com/astral-sh/ruff/pull/13616
synced_at: 2026-01-12T15:55:45Z
```

# Fix `PTH123` false positive when `open` is passed a file descriptor

---

_@zanieb_

Closes https://github.com/astral-sh/ruff/issues/12871

Includes some minor semantic type inference extensions changes to help with reliably detecting integers 

---

_Label `bug` added by @zanieb on 2024-10-03 17:53_

---

_Comment by @github-actions[bot] on 2024-10-03 18:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/replaceable_by_pathlib.rs`:171 on 2024-10-04 04:23_

nit: `is_file_descriptor` and I'd probably just inline `is_int_constant`

---

_@dhruvmanila approved on 2024-10-04 04:24_

---

_Merged by @zanieb on 2024-10-04 13:48_

---

_Closed by @zanieb on 2024-10-04 13:48_

---

_Branch deleted on 2024-10-04 13:48_

---
