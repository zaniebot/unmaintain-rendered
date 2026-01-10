```yaml
number: 6406
title: For Docker environments, a UV_PYTHON_DOWNLOADS env variable would be useful
type: issue
state: closed
author: hynek
labels:
  - enhancement
assignees: []
created_at: 2024-08-22T04:33:17Z
updated_at: 2024-08-23T05:14:28Z
url: https://github.com/astral-sh/uv/issues/6406
synced_at: 2026-01-10T04:45:09Z
```

# For Docker environments, a UV_PYTHON_DOWNLOADS env variable would be useful

---

_Issue opened by @hynek on 2024-08-22 04:33_

I'm aware of the [python-downloads option](https://docs.astral.sh/uv/#disabling-automatic-python-downloads), but in Docker builds, it's not always 100% clear what user runs what and when when, so it would be really nice if I could set `UV_PYTHON_DOWNLOADS=only-system` or `UV_NO_PYTHON_DOWNLOADS=1` once in my base image and not think about it anymore.

If any of this exists, I haven't found it neither by searching the docs nor by searching the code. 😬

---

_Label `enhancement` added by @konstin on 2024-08-22 08:35_

---

_Closed by @charliermarsh on 2024-08-22 12:52_

---

_Closed by @charliermarsh on 2024-08-22 12:52_

---

_Reopened by @zanieb on 2024-08-22 13:24_

---

_Assigned to @zanieb by @zanieb on 2024-08-22 13:24_

---

_Comment by @hynek on 2024-08-23 05:14_

Pretty sure this shipped in 0.3.2! 🎉

---

_Closed by @hynek on 2024-08-23 05:14_

---
