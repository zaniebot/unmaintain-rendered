```yaml
number: 6397
title: Assume Python 3.8 instead of 3.10 for target version
type: pull_request
state: merged
author: zanieb
labels:
  - breaking
  - configuration
assignees: []
merged: true
base: main
head: config/assume-38
created_at: 2023-08-07T16:49:38Z
updated_at: 2023-08-07T18:48:07Z
url: https://github.com/astral-sh/ruff/pull/6397
synced_at: 2026-01-12T15:55:21Z
```

# Assume Python 3.8 instead of 3.10 for target version

---

_@zanieb_

The target version should be the oldest supported version instead of an arbitary version. Since 3.7 is EOL, we should use 3.8. I would like to follow this up with more comprehensive default detection based on the environment.


---

_Label `breaking` added by @zanieb on 2023-08-07 16:49_

---

_Label `configuration` added by @zanieb on 2023-08-07 16:49_

---

_@charliermarsh approved on 2023-08-07 17:07_

---

_Comment by @charliermarsh on 2023-08-07 17:07_

Can you add a note to `BREAKING_CHANGES.md`?

---

_Comment by @zanieb on 2023-08-07 17:18_

Preview at https://github.com/astral-sh/ruff/blob/config/assume-38/BREAKING_CHANGES.md#00283

---

_Merged by @zanieb on 2023-08-07 18:48_

---

_Closed by @zanieb on 2023-08-07 18:48_

---

_Branch deleted on 2023-08-07 18:48_

---
