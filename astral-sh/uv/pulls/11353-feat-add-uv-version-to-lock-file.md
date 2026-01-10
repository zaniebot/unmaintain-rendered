```yaml
number: 11353
title: "feat: add uv version to lock file"
type: pull_request
state: closed
author: Aditya-PS-05
labels: []
assignees: []
base: main
head: feature/add-uv-version-to-lockfile
created_at: 2025-02-09T13:17:59Z
updated_at: 2025-03-03T02:43:26Z
url: https://github.com/astral-sh/uv/pull/11353
synced_at: 2026-01-10T11:10:36Z
```

# feat: add uv version to lock file

---

_Pull request opened by @Aditya-PS-05 on 2025-02-09 13:17_

Closes #11319

Add uv version information to uv.lock file to track which version of uv
was used to generate or update the lock file.

Before: 

![Screenshot from 2025-02-09 18-27-45](https://github.com/user-attachments/assets/24b4efdc-f275-40f0-afbe-ff58c933de7e)

After: 

![Screenshot from 2025-02-09 18-32-11](https://github.com/user-attachments/assets/92d21f6d-4a75-440f-a9ef-af055855caa4)

This makes it possible to:
- Track which uv version generated/modified the lock file

Current problem here: 
I didn't updated the snapshots and the tests which are failing, because I first want to make sure, I am currently solving the issue in a current way.

---

_Renamed from "feat: add uv version to lock file (#11319)" to "feat: add uv version to lock file" by @Aditya-PS-05 on 2025-02-09 13:21_

---

_Comment by @samypr100 on 2025-02-09 19:13_

Note, I'd considering getting the version from the `uv-version` crate rather than `CARGO_PKG_VERSION` 😄 

---

_Comment by @charliermarsh on 2025-03-03 02:43_

Thank you for the PR. Unfortunately I'm going to close this based on the decision we made in https://github.com/astral-sh/uv/issues/11319. Sorry about that!

---

_Closed by @charliermarsh on 2025-03-03 02:43_

---
