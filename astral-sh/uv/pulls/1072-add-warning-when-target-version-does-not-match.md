```yaml
number: 1072
title: Add warning when target version does not match build version
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
assignees: []
merged: true
base: main
head: zb/target-version-warn
created_at: 2024-01-23T23:16:12Z
updated_at: 2024-01-24T19:42:20Z
url: https://github.com/astral-sh/uv/pull/1072
synced_at: 2026-01-12T16:04:24Z
```

# Add warning when target version does not match build version

---

_@zanieb_

Follow-up to https://github.com/astral-sh/puffin/pull/1040 adding a user-facing warning when we cannot build with their requested version.

e.g.

```
❯ cargo run -- pip compile requirements.in --python-version 3.11.4 --no-build
Resolved 8 packages in 483ms
❯ cargo run -- pip compile requirements.in --python-version 3.11.4
warning: The requested Python version 3.11.4 is not available; 3.11.7 will be used to build dependencies instead.
Resolved 8 packages in 71ms
❯ cargo run -- pip compile requirements.in --python-version 3.11
Resolved 8 packages in 71ms
```

---

_@charliermarsh approved on 2024-01-24 00:29_

---

_Label `error messages` added by @charliermarsh on 2024-01-24 00:29_

---

_Comment by @charliermarsh on 2024-01-24 00:29_

Nice idea.

---

_Merged by @zanieb on 2024-01-24 19:42_

---

_Closed by @zanieb on 2024-01-24 19:42_

---

_Branch deleted on 2024-01-24 19:42_

---
