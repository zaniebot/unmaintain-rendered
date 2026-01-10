```yaml
number: 3398
title: Better error for unsupported Python version
type: pull_request
state: merged
author: hauntsaninja
labels:
  - error messages
assignees: []
merged: true
base: main
head: version-warning
created_at: 2024-05-06T08:25:03Z
updated_at: 2024-05-08T09:56:28Z
url: https://github.com/astral-sh/uv/pull/3398
synced_at: 2026-01-10T14:37:54Z
```

# Better error for unsupported Python version

---

_Pull request opened by @hauntsaninja on 2024-05-06 08:25_

Fixes #3371

It seems like uv doesn't proactively enforce 3.8+ and in most cases just issues a warning. This PR keeps that property, only adding the new check when it is known to fail. I checked the imports in this file and the other ones seem fine.

---

_Comment by @hauntsaninja on 2024-05-06 08:28_

Oh you know what? I think the Python 2 check is broken anyway, since uv now uses `-I` to run the interpreter. Yeah, looks like it'll just fail before we even get here for Python 3.3 and older :D

---

_Comment by @konstin on 2024-05-06 09:12_

I've confirmed that this fixes centos 7 (by failing with the correct error), thanks!

---

_Label `error messages` added by @konstin on 2024-05-06 09:12_

---

_Merged by @konstin on 2024-05-06 09:12_

---

_Closed by @konstin on 2024-05-06 09:12_

---

_Branch deleted on 2024-05-08 09:56_

---
