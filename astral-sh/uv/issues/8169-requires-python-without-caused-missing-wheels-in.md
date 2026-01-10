```yaml
number: 8169
title: "`requires-python` without `*` caused missing wheels in `uv.lock`"
type: issue
state: closed
author: ringsaturn
labels:
  - duplicate
  - question
assignees: []
created_at: 2024-10-14T08:54:06Z
updated_at: 2024-10-14T14:44:42Z
url: https://github.com/astral-sh/uv/issues/8169
synced_at: 2026-01-10T04:45:10Z
```

# `requires-python` without `*` caused missing wheels in `uv.lock`

---

_Issue opened by @ringsaturn on 2024-10-14 08:54_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

- uv: 0.4.20
- platform: Apple M3 Max
- cmd: `uv lock`

With config:

```toml
[project]
name = "demo"
version = "0.1.0"
readme = "README.md"

dependencies = [
    "pymongo==4.10.0",
]

requires-python = "==3.12"
```

Output:

<img width="1268" alt="image" src="https://github.com/user-attachments/assets/ccae5e68-9eee-4c74-a199-bdd7c974e812">

If use `requires-python = "==3.12.*"`, problem fixed.

Seems this came from https://github.com/astral-sh/uv/pull/7904


---

_Renamed from "`requires-python` without `*` cased missing wheels in `uv.lock`" to "`requires-python` without `*` caused missing wheels in `uv.lock`" by @ringsaturn on 2024-10-14 09:29_

---

_Comment by @zanieb on 2024-10-14 13:24_

Please see https://github.com/astral-sh/uv/issues/7426

---

_Label `duplicate` added by @zanieb on 2024-10-14 13:24_

---

_Label `question` added by @zanieb on 2024-10-14 13:24_

---

_Closed by @charliermarsh on 2024-10-14 13:47_

---

_Comment by @ringsaturn on 2024-10-14 13:49_

> Please see #7426

It's same root reason that uv hasn't define how a version without patch field is processed.

But caused unexpected behavior for wheel locking scenes. Maybe add `bug` label would be more appropriate.

---

_Comment by @zanieb on 2024-10-14 13:52_

> It's same root reason that uv hasn't define how a version without patch field is processed.

This behavior is not defined by us, it's a part of the Python standards and specifications — which we are following here.

---

_Comment by @ringsaturn on 2024-10-14 13:57_

> > It's same root reason that uv hasn't define how a version without patch field is processed.
> 
> This behavior is not defined by us, it's a part of the Python standards and specifications — which we are following here.

Thanks, looks like uv followed the [PEP 440](https://peps.python.org/pep-0440/) for this.

---

_Comment by @zanieb on 2024-10-14 14:44_

Yep! We're adding a warning for people who run into this.

---
