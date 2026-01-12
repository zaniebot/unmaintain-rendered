```yaml
number: 2341
title: "Remove `wheel` from default PEP 517 backend"
type: pull_request
state: merged
author: charliermarsh
labels:
  - compatibility
assignees: []
merged: true
base: main
head: charlie/wheel
created_at: 2024-03-10T16:16:39Z
updated_at: 2024-03-10T23:34:37Z
url: https://github.com/astral-sh/uv/pull/2341
synced_at: 2026-01-12T16:04:59Z
```

# Remove `wheel` from default PEP 517 backend

---

_@charliermarsh_

## Summary

This matches the latest `pip` and `build` releases. See: https://github.com/astral-sh/uv/issues/2313.

Closes https://github.com/astral-sh/uv/issues/2313.


---

_Label `compatibility` added by @charliermarsh on 2024-03-10 16:16_

---

_@konstin approved on 2024-03-10 16:31_

> Setuptools declares wheel as a wheel build dependency for PEP 517 builds already via a build hook

That's neat

---

_Comment by @charliermarsh on 2024-03-10 18:50_

I think we need to _keep_ `wheel` when do a legacy `setup.py` build.

---

_Merged by @charliermarsh on 2024-03-10 23:34_

---

_Closed by @charliermarsh on 2024-03-10 23:34_

---

_Branch deleted on 2024-03-10 23:34_

---
