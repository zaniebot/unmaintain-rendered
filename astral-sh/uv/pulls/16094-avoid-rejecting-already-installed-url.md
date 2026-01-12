```yaml
number: 16094
title: "Avoid rejecting already-installed URL distributions with `--no-sources`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/so
created_at: 2025-10-02T00:11:26Z
updated_at: 2025-10-02T13:32:16Z
url: https://github.com/astral-sh/uv/pull/16094
synced_at: 2026-01-12T16:12:06Z
```

# Avoid rejecting already-installed URL distributions with `--no-sources`

---

_@charliermarsh_

## Summary

This PR removes a guard that was accidentally included in https://github.com/astral-sh/uv/pull/15234/files#diff-6be6d80fe4821c47b70a372260f55e73b8da8182b8dcad7525d5cd3eb584532b. I meant to remove that logic before merging.

Closes https://github.com/astral-sh/uv/issues/16068.


---

_Label `bug` added by @charliermarsh on 2025-10-02 00:11_

---

_Review requested from @zanieb by @charliermarsh on 2025-10-02 00:11_

---

_Review requested from @konstin by @charliermarsh on 2025-10-02 00:11_

---

_Marked ready for review by @charliermarsh on 2025-10-02 00:11_

---

_@konstin approved on 2025-10-02 08:52_

---

_Merged by @charliermarsh on 2025-10-02 13:32_

---

_Closed by @charliermarsh on 2025-10-02 13:32_

---

_Branch deleted on 2025-10-02 13:32_

---
