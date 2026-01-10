---
number: 11497
title: What does --python-preference do for uv pip install
type: issue
state: closed
author: robinjhuang
labels:
  - question
assignees: []
created_at: 2025-02-13T23:19:58Z
updated_at: 2025-02-20T01:36:30Z
url: https://github.com/astral-sh/uv/issues/11497
synced_at: 2026-01-10T01:25:06Z
---

# What does --python-preference do for uv pip install

---

_Issue opened by @robinjhuang on 2025-02-13 23:19_

### Question

```
uv venv --python-preference only-system # this installs a system python
uv pip install --python-preference only-managed 
```

In this case, will uv replace my virtual env's python with a managed python? I tested on v0.5.31 and seems like it does not. Does the python-preference flag only come into play when there is no existing python environment? Would love some clarification on this.

### Platform

Windows 11 Pro Version	10.0.26100 Build 26100

### Version

uv 0.5.31 (e38ac4900 2025-02-12)

---

_Label `question` added by @robinjhuang on 2025-02-13 23:19_

---

_Comment by @zanieb on 2025-02-13 23:22_

> Does the python-preference flag only come into play when there is no existing python environment?

Yes.

We don't yet enforce the `python-preference` when interacting with existing virtual environments.

See

- https://github.com/astral-sh/uv/pull/7934
- #5144

---

_Comment by @robinjhuang on 2025-02-20 01:36_

Thanks for the clarification!

---

_Closed by @robinjhuang on 2025-02-20 01:36_

---
