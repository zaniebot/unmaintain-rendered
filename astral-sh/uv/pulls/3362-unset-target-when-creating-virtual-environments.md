```yaml
number: 3362
title: Unset target when creating virtual environments
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/target
created_at: 2024-05-03T23:08:38Z
updated_at: 2024-05-03T23:21:25Z
url: https://github.com/astral-sh/uv/pull/3362
synced_at: 2026-01-10T14:37:54Z
```

# Unset target when creating virtual environments

---

_Pull request opened by @charliermarsh on 2024-05-03 23:08_

## Summary

We were writing the build dependencies into the `--target` directory, which both made builds fail and led to them leaking into the user's directory.

Closes https://github.com/astral-sh/uv/issues/3349.



---

_Label `bug` added by @charliermarsh on 2024-05-03 23:08_

---

_Marked ready for review by @charliermarsh on 2024-05-03 23:08_

---

_@zanieb approved on 2024-05-03 23:10_

---

_Merged by @charliermarsh on 2024-05-03 23:21_

---

_Closed by @charliermarsh on 2024-05-03 23:21_

---

_Branch deleted on 2024-05-03 23:21_

---
