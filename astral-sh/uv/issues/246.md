```yaml
number: 246
title: Revisit decision to keep all wheels in-memory
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2023-10-30T22:28:56Z
updated_at: 2023-11-07T00:50:02Z
url: https://github.com/astral-sh/uv/issues/246
synced_at: 2026-01-10T05:40:31Z
```

# Revisit decision to keep all wheels in-memory

---

_Issue opened by @charliermarsh on 2023-10-30 22:28_

Right now, we keep all wheels in-memory when downloading-and-unzipping them. This needs to be stress tested... We may want to write to disk if the wheel exceeds a certain size.

---

_Added to milestone `Initial release` by @charliermarsh on 2023-10-30 22:29_

---

_Comment by @konstin on 2023-10-31 11:17_

torch is an ideal candidate, esp. since many users will not have that much free RAM

---

_Label `bug` added by @charliermarsh on 2023-11-01 21:26_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-06 05:31_

---

_Comment by @charliermarsh on 2023-11-06 05:31_

#323 incidentally makes this very easy to change.

---

_Removed from milestone `Initial release` by @charliermarsh on 2023-11-06 21:06_

---

_Added to milestone `Future` by @charliermarsh on 2023-11-06 21:06_

---

_Removed from milestone `Future` by @charliermarsh on 2023-11-06 21:06_

---

_Added to milestone `Initial release` by @charliermarsh on 2023-11-06 21:06_

---

_Closed by @charliermarsh on 2023-11-07 00:50_

---
