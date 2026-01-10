```yaml
number: 1139
title: Add arguments for pip-compile compatibility
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/allow-unsafe
created_at: 2024-01-26T21:23:36Z
updated_at: 2024-01-26T21:54:03Z
url: https://github.com/astral-sh/uv/pull/1139
synced_at: 2026-01-10T15:39:03Z
```

# Add arguments for pip-compile compatibility

---

_Pull request opened by @charliermarsh on 2024-01-26 21:23_

## Summary

This ensures that we warn when redundant options are passed (like `--allow-unsafe`, which is really common for forwards compatibility since it's going to be the default in a future release), and errors when known variants are passed that we _don't_ support (like `--resolver=backtracking`).

Closes https://github.com/astral-sh/puffin/issues/1127.

---

_Review requested from @zanieb by @charliermarsh on 2024-01-26 21:23_

---

_Comment by @charliermarsh on 2024-01-26 21:27_

I'm just gonna add the rest, I'll take out of draft when appropriate.

---

_Marked ready for review by @charliermarsh on 2024-01-26 21:34_

---

_@zanieb approved on 2024-01-26 21:39_

Mmmmm I like it

---

_Merged by @charliermarsh on 2024-01-26 21:54_

---

_Closed by @charliermarsh on 2024-01-26 21:54_

---

_Branch deleted on 2024-01-26 21:54_

---
