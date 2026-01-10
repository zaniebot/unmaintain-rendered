```yaml
number: 6482
title: "Restore `cache` suffix on Windows cache path"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - windows
  - cache
assignees: []
merged: true
base: main
head: charlie/receipt
created_at: 2024-08-23T01:21:36Z
updated_at: 2024-08-23T02:04:59Z
url: https://github.com/astral-sh/uv/pull/6482
synced_at: 2026-01-10T13:09:51Z
```

# Restore `cache` suffix on Windows cache path

---

_Pull request opened by @charliermarsh on 2024-08-23 01:21_

## Summary

We accidentally changed the Windows cache directory from `C:\Users\User\AppData\Local\uv\cache` to `C:\Users\User\AppData\Local\uv` in v0.3.0. We're considering this a bug, since it does _not_ match the documentation, and prior to v0.3.0, we always used the former. This PR migrates back to the previous location. It should be seamless for users, as we move the cache items to the new location on startup.

Closes https://github.com/astral-sh/uv/issues/6417.


---

_Label `bug` added by @charliermarsh on 2024-08-23 01:22_

---

_Label `windows` added by @charliermarsh on 2024-08-23 01:22_

---

_Label `cache` added by @charliermarsh on 2024-08-23 01:22_

---

_Comment by @charliermarsh on 2024-08-23 01:27_

Gonna test this on my Windows machine then include in the release.

---

_Comment by @charliermarsh on 2024-08-23 01:30_

Ah great, this failed on my Windows machine with an `Access is denied` error.

---

_Comment by @charliermarsh on 2024-08-23 01:41_

Ok, that was just because I had a mistake in the dir creation :)

---

_Merged by @charliermarsh on 2024-08-23 02:04_

---

_Closed by @charliermarsh on 2024-08-23 02:04_

---

_Branch deleted on 2024-08-23 02:04_

---
