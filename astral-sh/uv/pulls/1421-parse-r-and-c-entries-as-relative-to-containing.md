```yaml
number: 1421
title: "Parse `-r` and `-c` entries as relative to containing file"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/rel
created_at: 2024-02-16T03:57:10Z
updated_at: 2024-02-16T04:19:44Z
url: https://github.com/astral-sh/uv/pull/1421
synced_at: 2026-01-12T16:04:37Z
```

# Parse `-r` and `-c` entries as relative to containing file

---

_@charliermarsh_

## Summary

In a `requirements.txt` file, it turns out that the `-c` and `-r` entries should be interpreted as relative to the file in which they're declared, while the `-e` entries should be interpreted as relative to the current working directory, no matter where they're defined.

Previously, we always used the current working directory; now, we use the declaring file's directory for `-c` and `-r`.

Closes https://github.com/astral-sh/uv/issues/1367.

Closes https://github.com/astral-sh/uv/issues/1416.


---

_Label `bug` added by @charliermarsh on 2024-02-16 03:57_

---

_Review requested from @zanieb by @charliermarsh on 2024-02-16 03:57_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_compile.rs`:3323 on 2024-02-16 04:03_

Also verified that this fails on `main`.

---

_@charliermarsh reviewed on 2024-02-16 04:03_

---

_@zanieb approved on 2024-02-16 04:17_

---

_Merged by @charliermarsh on 2024-02-16 04:19_

---

_Closed by @charliermarsh on 2024-02-16 04:19_

---

_Branch deleted on 2024-02-16 04:19_

---
