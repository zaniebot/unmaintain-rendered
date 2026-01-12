```yaml
number: 2734
title: "Update macOS cache test to use `actions/setup-python`"
type: issue
state: closed
author: zanieb
labels:
  - good first issue
  - testing
assignees: []
created_at: 2024-03-30T14:19:44Z
updated_at: 2024-03-30T14:55:36Z
url: https://github.com/astral-sh/uv/issues/2734
synced_at: 2026-01-12T15:58:40Z
```

# Update macOS cache test to use `actions/setup-python`

---

_@zanieb_

No real reason for this test to use Brew's Python. It's probably faster to just install from the GitHub official setup-python action.

_Originally posted by @zanieb in https://github.com/astral-sh/uv/pull/2729#discussion_r1545394084_
            

---

_Label `good first issue` added by @zanieb on 2024-03-30 14:19_

---

_Label `testing` added by @zanieb on 2024-03-30 14:19_

---

_Renamed from "Update macOS cache test to use" to "Update macOS cache test to use `actions/setup-python`" by @zanieb on 2024-03-30 14:20_

---

_Comment by @zanieb on 2024-03-30 14:20_

This should be a simple change to the `ci.yaml` file and is a great way to make a first contribution to the project.

---

_Comment by @charliermarsh on 2024-03-30 14:23_

I thought it was intended to literally test Brew Python installs.

---

_Comment by @zanieb on 2024-03-30 14:30_

`system-test-macos-aarch64` is intended to cover a Brew Python install `cache-test-macos-aarch64` should be Python interpreter agnostic.

---

_Comment by @zanieb on 2024-03-30 14:55_

Sorry I did this in #2735 because CI was broken.

---

_Closed by @zanieb on 2024-03-30 14:55_

---
