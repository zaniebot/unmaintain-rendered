```yaml
number: 9945
title: Add symmetrical expressions to mutual incompatibilities
type: pull_request
state: closed
author: charliermarsh
labels:
  - enhancement
assignees: []
base: main
head: charlie/l
created_at: 2024-12-16T21:54:33Z
updated_at: 2024-12-18T15:29:50Z
url: https://github.com/astral-sh/uv/pull/9945
synced_at: 2026-01-10T12:00:01Z
```

# Add symmetrical expressions to mutual incompatibilities

---

_Pull request opened by @charliermarsh on 2024-12-16 21:54_

## Summary

For example, `sys_platform == 'linux' and platform_system != 'Linux'` is impossible.


---

_Label `enhancement` added by @charliermarsh on 2024-12-16 21:54_

---

_@zanieb approved on 2024-12-16 22:00_

---

_Comment by @charliermarsh on 2024-12-16 23:37_

Ugh. I don't know if we can actually do this, because on Android, `platform_system == 'Android'` but `sys_platform == 'linux'`.

---

_Comment by @charliermarsh on 2024-12-16 23:38_

Actually, that might not be true. I'm not sure where I learned / heard that.

---

_Comment by @charliermarsh on 2024-12-18 15:29_

Made redundant by https://github.com/astral-sh/uv/pull/9949.

---

_Closed by @charliermarsh on 2024-12-18 15:29_

---
