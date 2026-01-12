```yaml
number: 6655
title: "Read requirements from `requires.txt` when available"
type: pull_request
state: merged
author: charliermarsh
labels:
  - compatibility
assignees: []
merged: true
base: main
head: charlie/requires-txt
created_at: 2024-08-26T20:29:18Z
updated_at: 2024-08-27T13:02:27Z
url: https://github.com/astral-sh/uv/pull/6655
synced_at: 2026-01-12T16:07:28Z
```

# Read requirements from `requires.txt` when available

---

_@charliermarsh_

## Summary

Allows us to avoid building setuptools-based packages at versions prior to Metadata 2.2

Closes https://github.com/astral-sh/uv/issues/6647.


---

_Label `compatibility` added by @charliermarsh on 2024-08-26 20:29_

---

_Review requested from @zanieb by @charliermarsh on 2024-08-26 20:29_

---

_Review request for @zanieb removed by @charliermarsh on 2024-08-26 20:41_

---

_Review requested from @konstin by @charliermarsh on 2024-08-26 20:41_

---

_@konstin approved on 2024-08-27 08:10_

i'm not eager to read something as legacy as egg-info, but the code lgtm

---

_Merged by @charliermarsh on 2024-08-27 13:02_

---

_Closed by @charliermarsh on 2024-08-27 13:02_

---

_Branch deleted on 2024-08-27 13:02_

---
