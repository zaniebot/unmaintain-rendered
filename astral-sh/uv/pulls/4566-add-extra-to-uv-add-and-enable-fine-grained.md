```yaml
number: 4566
title: "Add `--extra` to `uv add` and enable fine grained updates"
type: pull_request
state: merged
author: ibraheemdev
labels: []
assignees: []
merged: true
base: main
head: ibraheem/uv-add-update
created_at: 2024-06-26T21:00:27Z
updated_at: 2024-06-26T22:36:08Z
url: https://github.com/astral-sh/uv/pull/4566
synced_at: 2026-01-12T16:06:19Z
```

# Add `--extra` to `uv add` and enable fine grained updates

---

_@ibraheemdev_

## Summary

- Adds a `--extra` flag to `uv add` that allows activating extras without the PEP508 syntax.
- `uv add` now errors if the update is ambiguous (e.g. the dependency is present twice with different markers)
- `uv add` is smarter about updates. For example, `uv add flask==3.0.0` followed by `uv add flask --extra dotenv` preserves the previous version specifier.

Resolves https://github.com/astral-sh/uv/issues/4419.


---

_Review requested from @charliermarsh by @ibraheemdev on 2024-06-26 21:01_

---

_Review requested from @zanieb by @ibraheemdev on 2024-06-26 21:01_

---

_@charliermarsh reviewed on 2024-06-26 21:05_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/pyproject_mut.rs`:222 on 2024-06-26 21:05_

Do we need to sort?

---

_@ibraheemdev reviewed on 2024-06-26 21:06_

---

_Review comment by @ibraheemdev on `crates/uv-distribution/src/pyproject_mut.rs`:222 on 2024-06-26 21:06_

I don't think so? This is updating `pyproject.toml` not the lockfile.

---

_@charliermarsh reviewed on 2024-06-26 21:09_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/pyproject_mut.rs`:222 on 2024-06-26 21:09_

But `dedup` only deduplicates adjacent entries, right?

---

_Review comment by @ibraheemdev on `crates/uv-distribution/src/pyproject_mut.rs`:222 on 2024-06-26 21:22_

Oh right, fixed. I wonder if `extras` should be a `HashSet`..

---

_@ibraheemdev reviewed on 2024-06-26 21:22_

---

_@charliermarsh approved on 2024-06-26 21:24_

---

_Merged by @ibraheemdev on 2024-06-26 22:36_

---

_Closed by @ibraheemdev on 2024-06-26 22:36_

---

_Branch deleted on 2024-06-26 22:36_

---
