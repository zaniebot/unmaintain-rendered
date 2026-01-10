```yaml
number: 8500
title: Improve error message for cache info serialization
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/s
created_at: 2024-10-23T13:07:51Z
updated_at: 2024-10-23T16:08:40Z
url: https://github.com/astral-sh/uv/pull/8500
synced_at: 2026-01-10T12:54:10Z
```

# Improve error message for cache info serialization

---

_Pull request opened by @charliermarsh on 2024-10-23 13:07_

## Summary

We no longer need this struct; we bumped the cache bucket version anyway, so the `Timestamp` variant is never encountered. This means we get real Serde error messages.

Closes https://github.com/astral-sh/uv/issues/8488.


---

_Label `error messages` added by @charliermarsh on 2024-10-23 13:07_

---

_Merged by @charliermarsh on 2024-10-23 13:17_

---

_Closed by @charliermarsh on 2024-10-23 13:17_

---

_Branch deleted on 2024-10-23 13:17_

---

_Comment by @adamtheturtle on 2024-10-23 16:08_

Nice!

---
