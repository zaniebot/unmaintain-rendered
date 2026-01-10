```yaml
number: 6821
title: "Pin Python version on `uv init`"
type: issue
state: closed
author: zanieb
labels:
  - projects
assignees: []
created_at: 2024-08-29T16:44:28Z
updated_at: 2024-09-03T23:43:51Z
url: https://github.com/astral-sh/uv/issues/6821
synced_at: 2026-01-10T04:45:09Z
```

# Pin Python version on `uv init`

---

_Issue opened by @zanieb on 2024-08-29 16:44_

See #6780 

Might want https://github.com/astral-sh/uv/issues/4970 first, but I don't think it's blocking.

Basically, we should pin the Python version for the project on init. We should allow opt-out with `--no-pin-python`.

---

_Label `good first issue` added by @zanieb on 2024-08-29 16:44_

---

_Label `projects` added by @zanieb on 2024-08-29 16:44_

---

_Comment by @charliermarsh on 2024-08-29 20:01_

For app and lib?

---

_Comment by @zanieb on 2024-08-29 20:23_

I think so — either way you want to develop on a consistent version.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-30 13:20_

---

_Comment by @samypr100 on 2024-08-30 16:05_

~~In the spirit of this comment https://github.com/astral-sh/uv/issues/6780#issuecomment-2316388155, wouldn't we want to opt-in with `--pin-python` instead of opting out? Even on `app` mode I typically see just minimum major.minor on the requires-python metadata field where as `.python-version` does contain a pinned version.~~

nvm, I realized this issue refers to `.python-version` and not `requires-python`.

---

_Label `good first issue` removed by @charliermarsh on 2024-09-03 19:29_

---

_Closed by @charliermarsh on 2024-09-03 23:43_

---
