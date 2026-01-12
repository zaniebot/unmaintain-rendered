```yaml
number: 2966
title: "Update hashes without `--upgrade` if not present"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/gen
created_at: 2024-04-10T14:45:02Z
updated_at: 2024-04-10T14:56:35Z
url: https://github.com/astral-sh/uv/pull/2966
synced_at: 2026-01-12T16:05:20Z
```

# Update hashes without `--upgrade` if not present

---

_@charliermarsh_

## Summary

If the user runs with `--generate-hashes`, and the lockfile doesn't contain _any_ hashes for a package (despite being pinned), we should add new hashes. This mirrors running `uv pip compile --generate-hashes` for the first time with an existing lockfile.

Closes #2962.


---

_Marked ready for review by @charliermarsh on 2024-04-10 14:46_

---

_Label `bug` added by @charliermarsh on 2024-04-10 14:46_

---

_Renamed from "Update hashes without --upgrade if not present" to "Update hashes without `--upgrade` if not present" by @charliermarsh on 2024-04-10 14:46_

---

_@zanieb approved on 2024-04-10 14:47_

---

_Merged by @charliermarsh on 2024-04-10 14:56_

---

_Closed by @charliermarsh on 2024-04-10 14:56_

---

_Branch deleted on 2024-04-10 14:56_

---
