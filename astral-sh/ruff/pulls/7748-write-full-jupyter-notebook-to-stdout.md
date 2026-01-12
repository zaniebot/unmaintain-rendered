```yaml
number: 7748
title: "Write full Jupyter notebook to `stdout`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - cli
assignees: []
merged: true
base: main
head: charlie/write
created_at: 2023-10-01T20:43:59Z
updated_at: 2023-10-02T14:26:35Z
url: https://github.com/astral-sh/ruff/pull/7748
synced_at: 2026-01-12T15:55:24Z
```

# Write full Jupyter notebook to `stdout`

---

_@charliermarsh_

## Summary

When writing back notebooks via `stdout`, we need to write back the entire JSON content, not _just_ the fixed source code. Otherwise, writing the output _back_ to the file will yield an invalid notebook.

Closes https://github.com/astral-sh/ruff/issues/7747

## Test Plan

`cargo test`


---

_Review requested from @dhruvmanila by @charliermarsh on 2023-10-01 20:44_

---

_Label `bug` added by @charliermarsh on 2023-10-01 20:44_

---

_Label `cli` added by @charliermarsh on 2023-10-01 20:44_

---

_Comment by @github-actions[bot] on 2023-10-01 20:58_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Review comment by @konstin on `crates/ruff_linter/src/source_kind.rs`:29 on 2023-10-02 08:04_

```suggestion
    /// Returns the python code for this source kind.
```
I'm trying to make it obvious that we're not talking about the json as source code of a jupyter notebook. I thought about "extracted" but that doesn't make sense for a plain text file.

---

_Review comment by @konstin on `crates/ruff_linter/src/source_kind.rs`:37 on 2023-10-02 08:05_

```suggestion
    /// Write the transformed source file.
```

---

_@konstin approved on 2023-10-02 08:07_

---

_@dhruvmanila approved on 2023-10-02 11:13_

Thanks! I think the `--diff` flag for stdout needs to be updated as well for Jupyter Notebooks but doesn't necessarily need to be done in this PR.

---

_Comment by @charliermarsh on 2023-10-02 13:56_

It might be fine for the `--diff` flag to show source code diffs -- I could see either being correct.

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/source_kind.rs`:29 on 2023-10-02 13:56_

Thanks!

---

_@charliermarsh reviewed on 2023-10-02 13:56_

---

_Merged by @charliermarsh on 2023-10-02 14:20_

---

_Closed by @charliermarsh on 2023-10-02 14:20_

---

_Branch deleted on 2023-10-02 14:20_

---
