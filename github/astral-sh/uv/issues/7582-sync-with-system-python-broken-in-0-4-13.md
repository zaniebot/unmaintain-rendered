---
number: 7582
title: Sync with system python broken in 0.4.13
type: issue
state: closed
author: b-phi
labels:
  - bug
assignees: []
created_at: 2024-09-20T11:56:21Z
updated_at: 2024-09-20T17:44:23Z
url: https://github.com/astral-sh/uv/issues/7582
synced_at: 2026-01-07T13:12:17-06:00
---

# Sync with system python broken in 0.4.13

---

_Issue opened by @b-phi on 2024-09-20 11:56_

For some use cases, I had been using `UV_PROJECT_ENVIRONMENT=/usr/local` to sync with the system python based on this comment https://github.com/astral-sh/uv/pull/6834#issuecomment-2319253359. With 0.4.13, this now breaks with 
> Project virtual environment directory `/usr/local/` cannot be used because it is not a virtual environment and is non-empty

Which was added in https://github.com/astral-sh/uv/pull/7527.

I've looked through the docs, but can't find any official recommendation for what path to use to sync with the system python `/usr/local/bin/python`.


---

_Comment by @zanieb on 2024-09-20 14:14_

Sorry about this. The problem is #7522 

I have a fix up in #7585 

---

_Label `bug` added by @zanieb on 2024-09-20 14:14_

---

_Assigned to @zanieb by @zanieb on 2024-09-20 14:14_

---

_Comment by @zanieb on 2024-09-20 17:40_

This will be fixed in the next release (sometime today)

New integration test in #7591

---

_Closed by @zanieb on 2024-09-20 17:40_

---

_Comment by @b-phi on 2024-09-20 17:44_

Thank you!

---
