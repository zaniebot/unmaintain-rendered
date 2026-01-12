```yaml
number: 2855
title: Include LICENSE files in source distribution
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/sdist
created_at: 2024-04-07T00:09:19Z
updated_at: 2024-04-07T00:42:31Z
url: https://github.com/astral-sh/uv/pull/2855
synced_at: 2026-01-12T16:05:15Z
```

# Include LICENSE files in source distribution

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/2724.

## Test Plan

Ran `maturin sdist`:

![Screenshot 2024-04-06 at 8 08 38 PM](https://github.com/astral-sh/uv/assets/1309177/48ef10d2-407a-4280-b7c0-e38591a28317)


---

_Label `bug` added by @charliermarsh on 2024-04-07 00:09_

---

_Marked ready for review by @charliermarsh on 2024-04-07 00:09_

---

_@zanieb approved on 2024-04-07 00:19_

---

_Comment by @charliermarsh on 2024-04-07 00:33_

So confused as to why this works locally but the schema is rejected in CI.

---

_Merged by @charliermarsh on 2024-04-07 00:42_

---

_Closed by @charliermarsh on 2024-04-07 00:42_

---

_Branch deleted on 2024-04-07 00:42_

---
