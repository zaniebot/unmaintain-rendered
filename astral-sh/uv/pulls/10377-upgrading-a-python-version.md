```yaml
number: 10377
title: Upgrading a Python version
type: pull_request
state: closed
author: simonw
labels:
  - documentation
assignees: []
base: main
head: patch-2
created_at: 2025-01-07T20:37:03Z
updated_at: 2025-05-27T16:26:17Z
url: https://github.com/astral-sh/uv/pull/10377
synced_at: 2026-01-12T16:09:16Z
```

# Upgrading a Python version

---

_@simonw_

## Summary

I couldn't figure out how to upgrade my previous 3.13.0rc2 version. Charlie helped me out: Tip via https://twitter.com/charliermarsh/status/1876696188130394372

## Test Plan

I tried running this command myself and it worked.

---

_Comment by @zanieb on 2025-01-07 22:31_

There are some caveats we'd need to document here, like this will break existing tool installations (and other virtual environments) that depend on the version. You'd be better off doing `uv python install 3.13.X` to add the new patch version in addition to the existing one.

---

_Assigned to @zanieb by @zanieb on 2025-01-07 22:31_

---

_Label `documentation` added by @samypr100 on 2025-01-08 02:47_

---

_Comment by @simonw on 2025-01-08 04:40_

Adding those caveats sounds necessary - I won't work on this any more, but I'd be delighted to see an improved version of this land written by someone who understands this command more than I do!

---

_Comment by @zanieb on 2025-01-08 04:50_

I can own that

---

_Comment by @zanieb on 2025-05-27 16:26_

I think I'll close this in favor of the dedicated command we're adding in #13312 

---

_Closed by @zanieb on 2025-05-27 16:26_

---
