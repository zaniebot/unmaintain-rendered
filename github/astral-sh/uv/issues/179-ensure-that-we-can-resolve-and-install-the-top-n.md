---
number: 179
title: Ensure that we can resolve and install the top N PyPI packages
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2023-10-24T19:30:56Z
updated_at: 2024-02-02T20:01:11Z
url: https://github.com/astral-sh/uv/issues/179
synced_at: 2026-01-07T13:12:16-06:00
---

# Ensure that we can resolve and install the top N PyPI packages

---

_Issue opened by @charliermarsh on 2023-10-24 19:30_

This should include some kind of automated testing, triaging failures, and then addressing those failures.

---

_Label `bug` added by @charliermarsh on 2023-10-24 19:31_

---

_Added to milestone `Initial release` by @charliermarsh on 2023-10-24 19:31_

---

_Assigned to @konstin by @charliermarsh on 2023-10-25 04:26_

---

_Comment by @konstin on 2023-11-21 16:18_

The resolve part is done, the install part is tbd

---

_Comment by @konstin on 2023-11-24 11:32_

With our current setup, installing is surprisingly much effort

---

_Comment by @charliermarsh on 2023-11-24 12:21_

What do you mean?

---

_Comment by @konstin on 2023-11-24 12:22_

I tried to replicate `resolve_many` as `install_many`, but stopped because it took too much effort trying to port functionality from `pip-sync` to a "just this package, top version only, 8k times, in parallel". I'd prioritize installing interesting resolutions.

---

_Comment by @charliermarsh on 2023-11-24 12:23_

Ohhh I see, it’s effort to write out the code. That makes sense.

---

_Unassigned @konstin by @konstin on 2023-11-27 10:23_

---

_Assigned to @konstin by @konstin on 2023-12-19 21:19_

---

_Comment by @konstin on 2024-02-02 20:01_

Done

---

_Closed by @konstin on 2024-02-02 20:01_

---
