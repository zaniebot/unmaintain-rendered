---
number: 11986
title: "Add support for constraints in `uv add`"
type: issue
state: closed
author: zanieb
labels:
  - enhancement
assignees: []
created_at: 2025-03-05T19:30:39Z
updated_at: 2025-03-17T21:27:34Z
url: https://github.com/astral-sh/uv/issues/11986
synced_at: 2026-01-07T13:12:18-06:00
---

# Add support for constraints in `uv add`

---

_Issue opened by @zanieb on 2025-03-05 19:30_

### Summary

`uv add` should accept a constraints file — which can be used to convert from `requirements.in` / `requirements.txt` to `uv.lock`.

### Example

See https://gist.github.com/zanieb/96b2825e6cec3bcc2b649aa02d43c40c for the current experience

The new experience would be a one-line `uv add -r requirements.in -c requirements.txt`

---

_Label `enhancement` added by @zanieb on 2025-03-05 19:30_

---

_Referenced in [astral-sh/uv#5200](../../astral-sh/uv/issues/5200.md) on 2025-03-05 19:33_

---

_Comment by @charliermarsh on 2025-03-06 03:45_

Just to clarify, the constraints provided via `-c` would _not_ be written to the `pyproject.toml`, right? (That's my understanding from the linked Gist.)

---

_Comment by @zanieb on 2025-03-06 04:46_

I think so? For import purposes, there's no reason to retain them. Maybe we only drop them if they are _pins_ since they're redundant?

An interesting case is like `uv add -c <file>` without any packages?

---

_Comment by @tkossak on 2025-03-06 10:24_

This feature would allow us to install [airflow](https://airflow.apache.org/docs/apache-airflow/2.6.3/installation/installing-from-pypi.html) module. With pip we do it via:
```
pip install "apache-airflow[celery]==2.6.3" --constraint "https://raw.githubusercontent.com/apache/airflow/constraints-2.6.3/constraints-3.8.txt"
```

But since uv doesn't support constraints, it's harder to manually check/replace modules with versions from constraint file.

---

_Comment by @charliermarsh on 2025-03-06 13:53_

> I think so? For import purposes, there's no reason to retain them. Maybe we only drop them if they are pins since they're redundant?

Yeah I don't think we should add them.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-17 00:37_

---

_Referenced in [astral-sh/uv#12209](../../astral-sh/uv/pulls/12209.md) on 2025-03-17 00:37_

---

_Closed by @charliermarsh on 2025-03-17 21:27_

---
