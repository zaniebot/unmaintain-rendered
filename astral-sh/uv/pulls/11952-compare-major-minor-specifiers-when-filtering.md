```yaml
number: 11952
title: Compare major-minor specifiers when filtering interpreters
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/rels
created_at: 2025-03-04T15:12:13Z
updated_at: 2025-03-04T23:29:40Z
url: https://github.com/astral-sh/uv/pull/11952
synced_at: 2026-01-10T11:10:39Z
```

# Compare major-minor specifiers when filtering interpreters

---

_Pull request opened by @charliermarsh on 2025-03-04 15:12_

## Summary

If we're looking at (e.g.) `python3.12`, and we have a `requires-python: ">=3.12.7, <3.13"`, then checking if the range includes `3.12` will return `false`. Instead, we need to look at the lower- and upper-bound major-minors of the `requires-python`.

Closes https://github.com/astral-sh/uv/issues/11825.


---

_Review requested from @zanieb by @charliermarsh on 2025-03-04 15:12_

---

_Label `bug` added by @charliermarsh on 2025-03-04 15:12_

---

_Converted to draft by @charliermarsh on 2025-03-04 15:15_

---

_Marked ready for review by @charliermarsh on 2025-03-04 15:16_

---

_@zanieb reviewed on 2025-03-04 15:57_

Can you add a `python_find` test case?

---

_@zanieb reviewed on 2025-03-04 18:29_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_find.rs`:677 on 2025-03-04 18:29_

You can use `context.python_versions.first().unwrap().1`

---

_@zanieb reviewed on 2025-03-04 18:41_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_find.rs`:689 on 2025-03-04 18:41_

As a note, you can just use this in `python find` directly instead of using a `pyproject.toml` to avoid indirection. I don't have strong feelings otherwise.

---

_@zanieb approved on 2025-03-04 18:41_

---

_@charliermarsh reviewed on 2025-03-04 23:20_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/python_find.rs`:689 on 2025-03-04 23:20_

Thanks!

---

_@charliermarsh reviewed on 2025-03-04 23:20_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/python_find.rs`:677 on 2025-03-04 23:20_

Great, thank you!

---

_Merged by @charliermarsh on 2025-03-04 23:29_

---

_Closed by @charliermarsh on 2025-03-04 23:29_

---

_Branch deleted on 2025-03-04 23:29_

---
