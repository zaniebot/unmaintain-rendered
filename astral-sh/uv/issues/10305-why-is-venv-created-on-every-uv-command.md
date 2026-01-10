---
number: 10305
title: "Why is `.venv` created on every `uv` command invocation despite the environment being available?"
type: issue
state: closed
author: aaqibb13
labels:
  - question
assignees: []
created_at: 2025-01-05T19:03:21Z
updated_at: 2025-03-03T03:21:53Z
url: https://github.com/astral-sh/uv/issues/10305
synced_at: 2026-01-10T01:24:52Z
---

# Why is `.venv` created on every `uv` command invocation despite the environment being available?

---

_Issue opened by @aaqibb13 on 2025-01-05 19:03_

If I am creating a virtual environment using `uv` via the following: 
```
uv venv venv
```
and then activating the virtual environment it via: 
```
source venv/bin/activate
```

Shouldn't the already created virtual environment (`venv` in this case) be referenced on the subsequent `uv` command invokations?
Why is the `.venv` still created?

As per the [docs](https://docs.astral.sh/uv/pip/environments/#using-a-virtual-environment): 
If the `venv` directory is not PEP 405 compliant, that's when the already present virtual environment will be ignored.
But if my `venv ` is already PEP 405 complaint since I created it via `uv` itself, why is this happening?
Am I missing something here?

---

_Comment by @zanieb on 2025-01-05 19:07_

Yeah, we don't respect activated environments in projects. See https://github.com/astral-sh/uv/pull/6834 and linked discussions

https://docs.astral.sh/uv/pip/environments/#using-a-virtual-environment is part of the `uv pip` documentation and specific to that subsection of uv.

See https://docs.astral.sh/uv/concepts/projects/layout/#the-project-environment for documentation on environments in projects.

---

_Label `question` added by @zanieb on 2025-01-05 19:07_

---

_Closed by @charliermarsh on 2025-03-03 03:21_

---
