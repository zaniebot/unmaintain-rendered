```yaml
number: 13513
title: "[nit] uv pip install lists updating, updated for git source"
type: issue
state: closed
author: wyattscarpenter
labels:
  - bug
assignees: []
created_at: 2025-05-18T13:43:24Z
updated_at: 2025-06-09T23:50:37Z
url: https://github.com/astral-sh/uv/issues/13513
synced_at: 2026-01-10T03:41:47Z
```

# [nit] uv pip install lists updating, updated for git source

---

_Issue opened by @wyattscarpenter on 2025-05-18 13:43_

### Summary

I have some git sources in my requirements.txt file, like so:

```
feedparser @ git+https://github.com/kurtmckee/feedparser@6c2a412a1569303c6c02d82ef38c08e19f81315e
```

When I run `uv pip install -r requirements.txt`, I get output like 

```
 Updating https://github.com/kurtmckee/feedparser (6c2a412a1569303c6c02d82ef38c08e19f81315e)
```

which then overwrites itself to become

```
 Updated https://github.com/kurtmckee/feedparser (6c2a412a1569303c6c02d82ef38c08e19f81315e)
```

This is not technically correct, because the dependency did not update. (I'm even pinned to the same commit hash!) It simply (I gather) checked if it should update, and then did not. So, more-accurate verbiage should be emitted, or perhaps no verbiage should be emitted.

### Platform

Windows 10

### Version

uv 0.7.5 (9d1a14e1f 2025-05-16)

### Python version

Python 3.12.8

---

_Label `bug` added by @wyattscarpenter on 2025-05-18 13:43_

---

_Comment by @charliermarsh on 2025-05-18 21:25_

Yeah I'd like to avoid emitting here when we already have the SHA checked-out.

---

_Assigned to @oconnor663 by @oconnor663 on 2025-05-29 21:00_

---

_Closed by @oconnor663 on 2025-06-09 23:50_

---
