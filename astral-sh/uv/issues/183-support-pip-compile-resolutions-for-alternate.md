```yaml
number: 183
title: "Support `pip-compile` resolutions for alternate platforms and Python versions"
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
  - wish
assignees: []
created_at: 2023-10-25T16:29:39Z
updated_at: 2024-01-23T02:55:05Z
url: https://github.com/astral-sh/uv/issues/183
synced_at: 2026-01-12T15:58:22Z
```

# Support `pip-compile` resolutions for alternate platforms and Python versions

---

_@charliermarsh_

E.g., could we support `puffin pip-compile --target-version py37` to do a Python 3.7-compatible resolution, even if your local Python is using some other version?


---

_Label `enhancement` added by @charliermarsh on 2023-10-25 16:29_

---

_Label `wish` added by @charliermarsh on 2023-10-25 16:29_

---

_Comment by @charliermarsh on 2023-10-25 16:29_

@konstin - What challenges would be involved here?

---

_Added to milestone `Feature complete` by @charliermarsh on 2023-10-25 16:30_

---

_Comment by @konstin on 2023-10-25 16:42_

For python version this is easy, just fake the relevant fields from https://peps.python.org/pep-0508/#environment-markers : `python_version`, `python_full_version`, `implementation_version` (potentially also `implementation_name` and `platform_python_implementation` for some of the worse cases).

For the platform, we need to collect sets of `implementation_name`, `os_name` and `sys_platform`. `platform_version` and `platform_release` are a bit tricky, tbh i think we could just fail the resolution if these need to be checked and we're on a pseudo-platform. (Note that if someone checks `platform_version` and `platform_release`, the decision isn't even portable between two machines with the same operating system).

---

_Comment by @charliermarsh on 2023-10-25 16:47_

Sounds good. This would be a really cool differentiating feature to support if easy.

---

_Comment by @charliermarsh on 2023-10-25 16:48_

Like even for our own projects I would love to have this. I hate that I have to use Python 3.7 to get a Python 3.7-compatible resolution.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-08 05:07_

---

_Comment by @zanieb on 2024-01-22 23:15_

@charliermarsh should we close this and track alternate platform support elsewhere?

---

_Comment by @charliermarsh on 2024-01-23 02:55_

Yeah, good call.

---

_Closed by @charliermarsh on 2024-01-23 02:55_

---
