```yaml
number: 6981
title: Move stdin formatting to its own command file
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/stdin
created_at: 2023-08-29T15:44:50Z
updated_at: 2023-08-29T16:12:28Z
url: https://github.com/astral-sh/ruff/pull/6981
synced_at: 2026-01-12T15:55:23Z
```

# Move stdin formatting to its own command file

---

_@charliermarsh_

## Summary

This is similar to `commands::check` vs. `commands::check_stdin`, and gets the logic out of the parent file (`lib.rs`). It also ensures that we avoid formatting files that should be excluded when `--force-exclude` is provided.


---

_Label `formatter` added by @charliermarsh on 2023-08-29 15:44_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-29 15:46_

---

_Review comment by @MichaReiser on `crates/ruff/src/fs.rs`:89 on 2023-08-29 15:50_

Can we move this to `ruff_cli`? It doesn't seem to be used by ruff itself. 

---

_@MichaReiser approved on 2023-08-29 15:51_

---

_@charliermarsh reviewed on 2023-08-29 15:52_

---

_Review comment by @charliermarsh on `crates/ruff/src/fs.rs`:89 on 2023-08-29 15:52_

I just realized we have the same method in the formatter too.

---

_Merged by @charliermarsh on 2023-08-29 16:06_

---

_Closed by @charliermarsh on 2023-08-29 16:06_

---

_Branch deleted on 2023-08-29 16:06_

---

_Comment by @github-actions[bot] on 2023-08-29 16:12_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
