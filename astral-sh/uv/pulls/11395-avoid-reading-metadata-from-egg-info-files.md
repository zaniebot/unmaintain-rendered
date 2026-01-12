```yaml
number: 11395
title: "Avoid reading metadata from `.egg-info` files"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: tracking/060
head: charlie/egg-info
created_at: 2025-02-10T18:57:15Z
updated_at: 2025-02-10T19:28:33Z
url: https://github.com/astral-sh/uv/pull/11395
synced_at: 2026-01-12T16:09:49Z
```

# Avoid reading metadata from `.egg-info` files

---

_@charliermarsh_

## Summary

We added this to help with resolving some specific packages, and for parity with Poetry. But in some cases, this metadata is just wrong, and at the very least it's unreliable.

Closes https://github.com/astral-sh/uv/issues/8989.

Closes #10945.


---

_Added to milestone `v0.6.0` by @charliermarsh on 2025-02-10 18:57_

---

_Label `bug` added by @charliermarsh on 2025-02-10 18:58_

---

_@konstin approved on 2025-02-10 19:03_

---

_Merged by @charliermarsh on 2025-02-10 19:28_

---

_Closed by @charliermarsh on 2025-02-10 19:28_

---

_Branch deleted on 2025-02-10 19:28_

---
