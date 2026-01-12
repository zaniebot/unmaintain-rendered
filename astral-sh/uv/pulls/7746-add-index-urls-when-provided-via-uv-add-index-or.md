```yaml
number: 7746
title: "Add index URLs when provided via `uv add --index` or `--default-index`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: charlie/index-api-uv-add
created_at: 2024-09-27T19:17:08Z
updated_at: 2024-10-15T22:57:27Z
url: https://github.com/astral-sh/uv/pull/7746
synced_at: 2026-01-12T16:07:58Z
```

# Add index URLs when provided via `uv add --index` or `--default-index`

---

_@charliermarsh_

## Summary

The behavior is as follows:

- If you provide `--index` or `--default-index` on the command-line, we add those indexes to the `pyproject.toml` (with names, if provided, as in `--index pytorch=https://download.pytorch.org/whl/cu121`.
- If you provide `--index-url` or `--default-index`, we warn, but don't add the indexes to the file. (This seems wrong -- why not add them?)
- If you provide an index with a name or URL that already exists, we remove that entry, and add the new index to the top of the list (since it now has highest priority).
- If you provide a `--default-index`, and an index already has `default = true`, we remove that entry, since it won't be used anymore.

We do _not_ pin packages to specific indexes yet.


---

_Marked ready for review by @charliermarsh on 2024-09-27 19:18_

---

_Label `enhancement` added by @charliermarsh on 2024-09-27 19:18_

---

_Label `cli` added by @charliermarsh on 2024-09-27 19:18_

---

_Review requested from @zanieb by @charliermarsh on 2024-09-28 19:02_

---

_Review requested from @konstin by @charliermarsh on 2024-09-28 19:02_

---

_Review comment by @konstin on `crates/uv/tests/edit.rs`:3710 on 2024-09-30 09:52_

I have a slight preference that indices should be named, at least when they are non-default

---

_@konstin approved on 2024-09-30 09:53_

---

_@charliermarsh reviewed on 2024-09-30 21:25_

---

_Review comment by @charliermarsh on `crates/uv/tests/edit.rs`:3710 on 2024-09-30 21:25_

As in, we should require it?

---

_@konstin reviewed on 2024-09-30 21:36_

---

_Review comment by @konstin on `crates/uv/tests/edit.rs`:3710 on 2024-09-30 21:36_

Yeah

---

_Review comment by @zanieb on `crates/uv/tests/edit.rs`:3710 on 2024-10-01 14:00_

What's the benefit of requiring it? It seems restrictive.

---

_@zanieb reviewed on 2024-10-01 14:00_

---

_@konstin reviewed on 2024-10-01 14:56_

---

_Review comment by @konstin on `crates/uv/tests/edit.rs`:3710 on 2024-10-01 14:56_

Consistency and the option to refer to an index by name everywhere

---

_@zanieb reviewed on 2024-10-01 14:58_

---

_Review comment by @zanieb on `crates/uv/tests/edit.rs`:3710 on 2024-10-01 14:58_

Oh you think we should require a name even when they're defined in the file manually? I'm not sure it's worth it. 

---

_Merged by @charliermarsh on 2024-10-15 22:57_

---

_Closed by @charliermarsh on 2024-10-15 22:57_

---

_Branch deleted on 2024-10-15 22:57_

---
