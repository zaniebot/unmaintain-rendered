---
number: 11397
title: "Flaky failure due to `setup-python` cache missing"
type: issue
state: closed
author: charliermarsh
labels:
  - testing
assignees: []
created_at: 2025-02-10T19:23:17Z
updated_at: 2025-02-10T20:36:02Z
url: https://github.com/astral-sh/uv/issues/11397
synced_at: 2026-01-07T13:12:18-06:00
---

# Flaky failure due to `setup-python` cache missing

---

_Issue opened by @charliermarsh on 2025-02-10 19:23_

In: https://github.com/astral-sh/uv/actions/runs/13248696681/job/36981416581?pr=11395

![Image](https://github.com/user-attachments/assets/fed91216-a1eb-41ef-aebd-50489067ab70)

---

_Label `testing` added by @charliermarsh on 2025-02-10 19:23_

---

_Comment by @zanieb on 2025-02-10 20:01_

This is annoying..  just a bug in `setup-python` itself?

---

_Comment by @charliermarsh on 2025-02-10 20:13_

Yeah, not sure? Feel free to close if you prefer -- just trying to be diligent in filing these when they pop up.

---

_Comment by @zanieb on 2025-02-10 20:14_

It's crazy this is a hard error?

---

_Comment by @zanieb on 2025-02-10 20:15_

Did the pip cache move? Aren't we installing things that should populate the cache?

---

_Comment by @charliermarsh on 2025-02-10 20:19_

![Image](https://github.com/user-attachments/assets/1c7038fc-4e02-4b38-906b-d918a853023f)

---

_Referenced in [astral-sh/uv#11402](../../astral-sh/uv/pulls/11402.md) on 2025-02-10 20:22_

---

_Comment by @zanieb on 2025-02-10 20:23_

https://github.com/pypa/pip/releases/tag/25.0.1 went out yesterday

---

_Comment by @zanieb on 2025-02-10 20:26_

However the diff is pretty sparse https://github.com/pypa/pip/compare/25.0...25.0.1

---

_Referenced in [astral-sh/uv#11403](../../astral-sh/uv/pulls/11403.md) on 2025-02-10 20:27_

---

_Comment by @zanieb on 2025-02-10 20:30_

Confused the cache ever existed here. We only use `python -m pip show` and all the installs are via `uv pip`.

Maybe a runner image change in https://github.com/actions/runner-images/releases/tag/win22%2F20250203.1

---

_Closed by @zanieb on 2025-02-10 20:36_

---
