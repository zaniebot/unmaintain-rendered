```yaml
number: 654
title: Ignore missing manifest entries in the built wheel cache
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/cache-entry
created_at: 2023-12-15T04:10:52Z
updated_at: 2023-12-15T17:24:11Z
url: https://github.com/astral-sh/uv/pull/654
synced_at: 2026-01-12T16:04:06Z
```

# Ignore missing manifest entries in the built wheel cache

---

_@charliermarsh_

## Summary

This is more of a hypothetical problem, but the cache manifest could in theory get out-of-sync with the contents on disk. This PR modifies the `BuiltWheelMetadata` lookup to warn (but not fail) if the manifest includes a wheel that no longer exists on disk. You can mimic this by removing a wheel from the `built-wheels-v0` cache without modifying the manifest correspondingly.

---

_Review requested from @konstin by @charliermarsh on 2023-12-15 04:10_

---

_@konstin approved on 2023-12-15 08:30_

---

_Comment by @konstin on 2023-12-15 08:31_

If we do metadata only builds this might be wanted though, for cases where only have and need the metadata but not the wheel

---

_Comment by @charliermarsh on 2023-12-15 17:07_

Yeah we'll need to revisit for sure.

---

_Label `bug` added by @charliermarsh on 2023-12-15 17:20_

---

_Merged by @charliermarsh on 2023-12-15 17:24_

---

_Closed by @charliermarsh on 2023-12-15 17:24_

---

_Branch deleted on 2023-12-15 17:24_

---
