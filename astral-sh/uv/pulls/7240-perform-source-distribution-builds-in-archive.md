```yaml
number: 7240
title: Perform source distribution builds in archive directory
type: pull_request
state: closed
author: charliermarsh
labels:
  - windows
  - cache
assignees: []
draft: true
base: main
head: charlie/build-cache
created_at: 2024-09-10T02:09:04Z
updated_at: 2024-09-23T23:29:30Z
url: https://github.com/astral-sh/uv/pull/7240
synced_at: 2026-01-12T16:07:45Z
```

# Perform source distribution builds in archive directory

---

_@charliermarsh_

## Summary

This is an attempt to help with https://github.com/astral-sh/uv/issues/7078 by reducing the length of cache paths on Windows. It needs some testing on my Windows machine though.


---

_Label `windows` added by @charliermarsh on 2024-09-10 02:09_

---

_Label `cache` added by @charliermarsh on 2024-09-10 02:09_

---

_Comment by @zanieb on 2024-09-10 02:17_

This is just something to help right? Not the "real" long term fix?

---

_Comment by @charliermarsh on 2024-09-10 02:19_

> Not the "real" long term fix?

Sorry, what would this be? There's a fundamental path length limit. All we can do is help, I don't think there is a real fix.


---

_Comment by @zanieb on 2024-09-10 02:32_

I was thinking about how we previously talked about using a hash instead of a wheel file name in the cache. Looking at the linked issue I see this looks like a separate file length issue than that though.

---

_Comment by @charliermarsh on 2024-09-10 02:34_

Oh sorry, yes! That's a separate issue, I don't think this will help with that sadly.

---

_Comment by @zanieb on 2024-09-10 02:36_

No problem I should have looked at the issue or read more closely. Thanks for clarifying!

---

_Closed by @charliermarsh on 2024-09-23 23:29_

---
