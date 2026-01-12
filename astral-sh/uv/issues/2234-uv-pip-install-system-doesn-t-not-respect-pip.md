```yaml
number: 2234
title: "`uv pip install --system` doesn't not respect `PIP_BREAK_SYSTEM_PACKAGES`"
type: issue
state: closed
author: jfcherng
labels:
  - compatibility
assignees: []
created_at: 2024-03-06T08:14:44Z
updated_at: 2024-03-06T20:37:30Z
url: https://github.com/astral-sh/uv/issues/2234
synced_at: 2026-01-12T15:58:36Z
```

# `uv pip install --system` doesn't not respect `PIP_BREAK_SYSTEM_PACKAGES`

---

_@jfcherng_

While `--system` has been added recently, as per [Externally Managed Environments](https://packaging.python.org/en/latest/specifications/externally-managed-environments/), if there is the `EXTERNALLY-MANAGED` marker file in the Python distribution, then packages won't be installed.

However, the env variable `PIP_BREAK_SYSTEM_PACKAGES` can make `pip` to forcely install to externally managed Python distribution. I try to migrate `pip` to `uv` but `uv` doesn't seem to respect `PIP_BREAK_SYSTEM_PACKAGES` so I guess there is just no way to skip that check. An workaround is that I just remove the `/usr/lib/python3.x/EXTERNALLY-MANAGED` file.

My use case is that I am making a read-only Singularity image (it's just like a Docker image). So I just don't care about that.

---

_Label `compatibility` added by @charliermarsh on 2024-03-06 13:47_

---

_Comment by @zanieb on 2024-03-06 16:56_

We're willing to add support for the command line flag, we just haven't done so yet.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-06 17:39_

---

_Closed by @charliermarsh on 2024-03-06 20:37_

---
