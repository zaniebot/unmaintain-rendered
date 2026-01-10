```yaml
number: 13437
title: Avoid parsing with Salsa
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/parse-db
created_at: 2024-09-21T13:14:51Z
updated_at: 2024-09-21T14:06:04Z
url: https://github.com/astral-sh/ruff/pull/13437
synced_at: 2026-01-10T21:30:32Z
```

# Avoid parsing with Salsa

---

_Pull request opened by @charliermarsh on 2024-09-21 13:14_

## Summary

For reasons I haven't investigated, this speeds up the resolver about 2x (from 6.404s to 3.612s on an extremely large codebase).

## Test Plan

\cc @BurntSushi 

```
[andrew@duff rippling]$ time ruff analyze graph --preview > /dev/null

real    3.274
user    16.039
sys     7.609
maxmem  11631 MB
faults  0
[andrew@duff rippling]$ time ruff-patch analyze graph --preview > /dev/null

real    1.841
user    14.625
sys     3.639
maxmem  7173 MB
faults  0
[andrew@duff rippling]$ time ruff-patch2 analyze graph --preview > /dev/null

real    2.087
user    15.333
sys     4.869
maxmem  8642 MB
faults  0
```

Where that's `main`, then (`ruff-patch`) using the version with no `File`, no `SemanticModel`, then (`ruff-patch2`) using `File`.


---

_Review comment by @charliermarsh on `crates/ruff_graph/src/resolver.rs`:22 on 2024-09-21 13:16_

@MichaReiser -- Would you prefer that I create a struct that exposes this? (I can't use `SemanticModel` because that requires a `File`, and I no longer have a `File` upfront.)

---

_@charliermarsh reviewed on 2024-09-21 13:16_

---

_Label `performance` added by @charliermarsh on 2024-09-21 13:47_

---

_Merged by @charliermarsh on 2024-09-21 13:52_

---

_Closed by @charliermarsh on 2024-09-21 13:52_

---

_Branch deleted on 2024-09-21 13:52_

---

_Comment by @github-actions[bot] on 2024-09-21 14:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
