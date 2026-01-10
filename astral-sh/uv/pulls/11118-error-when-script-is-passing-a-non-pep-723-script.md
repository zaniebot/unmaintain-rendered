```yaml
number: 11118
title: "Error when `--script` is passing a non-PEP 723 script"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/script-err
created_at: 2025-01-30T20:41:06Z
updated_at: 2025-01-30T20:50:00Z
url: https://github.com/astral-sh/uv/pull/11118
synced_at: 2026-01-10T11:10:34Z
```

# Error when `--script` is passing a non-PEP 723 script

---

_Pull request opened by @charliermarsh on 2025-01-30 20:41_

## Summary

We now show a custom error if (1) the file doesn't exist at all, or (2) it's not a PEP 723 script.

In the future, `uv lock --script` should probably initialize the script, but that requires a more extensive refactor. At present, we just silently lock the project instead, which is pretty bad!

Closes https://github.com/astral-sh/uv/issues/10979.


---

_Review requested from @Gankra by @charliermarsh on 2025-01-30 20:41_

---

_Review requested from @zanieb by @charliermarsh on 2025-01-30 20:41_

---

_Label `bug` added by @charliermarsh on 2025-01-30 20:41_

---

_@charliermarsh reviewed on 2025-01-30 20:42_

---

_Review comment by @charliermarsh on `crates/uv/src/lib.rs`:215 on 2025-01-30 20:42_

@zanieb -- Just calling out that I deferred this. It looks like a lot more work.

---

_@zanieb approved on 2025-01-30 20:44_

---

_Merged by @charliermarsh on 2025-01-30 20:49_

---

_Closed by @charliermarsh on 2025-01-30 20:49_

---

_Branch deleted on 2025-01-30 20:50_

---
