---
number: 12787
title: "uv bootstraps itself with `required-version`"
type: issue
state: closed
author: juftin
labels:
  - duplicate
  - enhancement
assignees: []
created_at: 2025-04-09T20:26:54Z
updated_at: 2025-04-09T20:56:13Z
url: https://github.com/astral-sh/uv/issues/12787
synced_at: 2026-01-07T13:12:18-06:00
---

# uv bootstraps itself with `required-version`

---

_Issue opened by @juftin on 2025-04-09 20:26_

### Summary

[required-version](https://docs.astral.sh/uv/reference/settings/#required-version) has been very useful for my team to ensure that we don't get bit by LockFile invalidations when we enable `UV_LOCKED` mode.

> [required-version](https://docs.astral.sh/uv/reference/settings/#required-version)
>>Enforce a requirement on the version of uv.
>>
>>If the version of uv does not meet the requirement at runtime, uv will exit with an error.
>>
>> Accepts a [PEP 440](https://peps.python.org/pep-0440/) specifier, like ==0.5.0 or >=0.5.0.

Rather than exiting with an error, it would be very useful if `uv` was able to bootstrap itself with a compatible version of `uv` to run the command with (likely via `uvx`)

### Example

You have `uv==0.4.0` on your local machine and the following `pyproject.toml`:
```toml
[project]
name = "example"
version = "1.2.3"
description = "Example Project Description"
readme = "README.md"
requires-python = ">=3.11,<3.12"
dependencies = [
    "apache-airflow==2.8.0"
]

[tool.uv]
required-version = ">=0.6.0,<0.7.0"
```

Rather than raising an error that your `uv` version doesn't comply with the `required-version` when running `uv lock`, `uv` would instead essentially run `uvx "uv>=0.5.0,<0.6.0" lock`

---

_Label `enhancement` added by @juftin on 2025-04-09 20:26_

---

_Comment by @zanieb on 2025-04-09 20:47_

There are other issues asking for this already. Sounds cool though :)

---

_Label `duplicate` added by @zanieb on 2025-04-09 20:48_

---

_Comment by @juftin on 2025-04-09 20:52_

> There are other issues asking for this already. Sounds cool though :)

Dang - I searched for them and didn't see anything. Sorry about the duplicate noise, I'll try to track them down. 

---

_Comment by @juftin on 2025-04-09 20:55_

Duplicates https://github.com/astral-sh/uv/issues/11065
Similar to https://github.com/astral-sh/uv/issues/10547

---

_Closed by @juftin on 2025-04-09 20:55_

---

_Comment by @zanieb on 2025-04-09 20:56_

Thank you! Appreciate it. I don't blame you for GitHub's janky search :)

---
