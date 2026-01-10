```yaml
number: 10646
title: "Remove use of expensive package `babel` in `uv tool` tests"
type: issue
state: closed
author: zanieb
labels:
  - help wanted
  - testing
assignees: []
created_at: 2025-01-15T21:09:04Z
updated_at: 2025-01-21T17:26:28Z
url: https://github.com/astral-sh/uv/issues/10646
synced_at: 2026-01-10T04:27:58Z
```

# Remove use of expensive package `babel` in `uv tool` tests

---

_Issue opened by @zanieb on 2025-01-15 21:09_

`babel` is used frequently in the tool upgrade and install tests, but it's ~10MB (opposed to e.g., `anyio` which is 200kB). These tests are showing up on the slow list (https://github.com/astral-sh/uv/issues/878#issuecomment-2593922536).

---

_Label `help wanted` added by @zanieb on 2025-01-15 21:09_

---

_Label `testing` added by @zanieb on 2025-01-15 21:09_

---

_Comment by @zanieb on 2025-01-15 21:09_

I'm not sure why we're using `babel` and what we should replace it with.

---

_Comment by @charliermarsh on 2025-01-17 19:00_

It might be in part because the executable name is different than the package name? But I don't know either.

---

_Comment by @charliermarsh on 2025-01-17 19:01_

Should I publish a small executable package for this? (We then have to change `UV_EXCLUDE_NEWER` for those tests which is tedious but ~fine.)

---

_Comment by @zanieb on 2025-01-17 19:01_

Yeah that'd be great, imo.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-17 23:29_

---

_Closed by @charliermarsh on 2025-01-21 17:26_

---
