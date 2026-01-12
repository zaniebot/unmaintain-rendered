```yaml
number: 4526
title: "Read persistent configuration from non-workspace `pyproject.toml`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - configuration
assignees: []
merged: true
base: main
head: charlie/poetry
created_at: 2024-06-25T18:13:23Z
updated_at: 2024-06-25T18:53:15Z
url: https://github.com/astral-sh/uv/pull/4526
synced_at: 2026-01-12T16:06:17Z
```

# Read persistent configuration from non-workspace `pyproject.toml`

---

_@charliermarsh_

## Summary

If the user puts their configuration in a `pyproject.toml` that _isn't_ a valid workspace root (e.g., it's a Poetry file), we won't discover it, because we only look in `uv.toml` files in that case. I think this is somewhat debatable... We could choose to _require_ `uv.toml` there, but as a user I'd probably expect it to work?

Closes https://github.com/astral-sh/uv/issues/4521.


---

_Label `configuration` added by @charliermarsh on 2024-06-25 18:13_

---

_Marked ready for review by @charliermarsh on 2024-06-25 18:13_

---

_Review requested from @zanieb by @charliermarsh on 2024-06-25 18:13_

---

_Review requested from @konstin by @charliermarsh on 2024-06-25 18:13_

---

_@zanieb reviewed on 2024-06-25 18:32_

---

_Review comment by @zanieb on `crates/uv-settings/src/lib.rs`:72 on 2024-06-25 18:32_

Did you mean to drop this? Does this happen elsewhere now somehow?

---

_@zanieb approved on 2024-06-25 18:32_

Yeah I think this behavior makes sense, though it's annoying.

---

_Review comment by @charliermarsh on `crates/uv-settings/src/lib.rs`:72 on 2024-06-25 18:34_

Yeah this is in `from_directory`.

---

_@charliermarsh reviewed on 2024-06-25 18:34_

---

_Comment by @charliermarsh on 2024-06-25 18:41_

I'll add a test too.

---

_Merged by @charliermarsh on 2024-06-25 18:53_

---

_Closed by @charliermarsh on 2024-06-25 18:53_

---

_Branch deleted on 2024-06-25 18:53_

---
