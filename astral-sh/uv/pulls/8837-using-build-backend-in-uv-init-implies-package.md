```yaml
number: 8837
title: "Using `--build-backend` in `uv init` implies `--package`"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/build-backend-package
created_at: 2024-11-05T19:02:21Z
updated_at: 2024-11-06T01:27:10Z
url: https://github.com/astral-sh/uv/pull/8837
synced_at: 2026-01-12T16:08:31Z
```

# Using `--build-backend` in `uv init` implies `--package`

---

_@zanieb_

_No description provided._

---

_Label `bug` added by @zanieb on 2024-11-05 19:02_

---

_Merged by @zanieb on 2024-11-05 20:15_

---

_Closed by @zanieb on 2024-11-05 20:15_

---

_Branch deleted on 2024-11-05 20:15_

---

_Comment by @j178 on 2024-11-06 01:07_

Just curious, doesn't https://github.com/astral-sh/uv/pull/8593 already address this? 🤔

---

_Comment by @zanieb on 2024-11-06 01:24_

That's a great question — maybe I managed to not be on the latest version? I thought I checked `main` though. Will double check.

---

_Comment by @zanieb on 2024-11-06 01:26_

I reverted this commit and it looks like it does work as intended. Not sure what happened locally. Thanks!

---

_Label `bug` removed by @zanieb on 2024-11-06 01:27_

---

_Label `internal` added by @zanieb on 2024-11-06 01:27_

---
