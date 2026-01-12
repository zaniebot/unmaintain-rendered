```yaml
number: 6882
title: Use lower-bound semantics for all Python compatibility comparisons
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/lower-compat
created_at: 2024-08-30T18:27:37Z
updated_at: 2024-09-02T18:23:43Z
url: https://github.com/astral-sh/uv/pull/6882
synced_at: 2026-01-12T16:07:34Z
```

# Use lower-bound semantics for all Python compatibility comparisons

---

_@charliermarsh_

## Summary

Right now, we have slightly different `requires-python` semantics for `-p 3.11` vs. `-p 3.11 --universal`, and slightly different (wrong) semantics for how we compare against the _installed_ Python version (which doesn't ignore upper bounds, but should).

This PR rips it all out and replaces it with consistent semantics across `uv lock`, `uv pip compile -p 3.11`, and `uv pip compile -p 3.11 --universal`. We now always ignore upper bounds.

Closes https://github.com/astral-sh/uv/issues/6859.

Closes https://github.com/astral-sh/uv/issues/5045.


---

_Label `bug` added by @charliermarsh on 2024-08-30 18:27_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-08-30 18:29_

---

_Review requested from @konstin by @charliermarsh on 2024-08-30 18:29_

---

_@charliermarsh reviewed on 2024-08-30 18:30_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_compile.rs`:6486 on 2024-08-30 18:30_

The simplification of markers here is somewhat debatable. I might rather _not_ do this after Andrew's PR merges, since we don't record a `requires-python` in these files.

---

_Marked ready for review by @charliermarsh on 2024-08-30 18:35_

---

_Review comment by @konstin on `crates/uv-resolver/src/python_requirement.rs`:16 on 2024-09-02 07:30_

CC @burntsushi, this fixes the requires-python-is-none in your PR

---

_Review comment by @konstin on `crates/uv-resolver/src/resolution/graph.rs`:164 on 2024-09-02 07:37_

Is that still relevant?

---

_@konstin approved on 2024-09-02 07:40_

---

_Merged by @charliermarsh on 2024-09-02 18:23_

---

_Closed by @charliermarsh on 2024-09-02 18:23_

---

_Branch deleted on 2024-09-02 18:23_

---
