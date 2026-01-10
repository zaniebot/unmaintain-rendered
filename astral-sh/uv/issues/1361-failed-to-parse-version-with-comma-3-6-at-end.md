```yaml
number: 1361
title: "Failed to parse version with 'comma', `>=3.6,` at end"
type: issue
state: closed
author: onjin
labels:
  - bug
assignees: []
created_at: 2024-02-15T21:40:05Z
updated_at: 2024-02-16T02:08:06Z
url: https://github.com/astral-sh/uv/issues/1361
synced_at: 2026-01-10T05:40:31Z
```

# Failed to parse version with 'comma', `>=3.6,` at end

---

_Issue opened by @onjin on 2024-02-15 21:40_

https://pypi.org/simple/openpyxl/?format=application/vnd.pypi.simple.v1+json

```
uv pip install openpyxl==3.0.5
error: Failed to download: openpyxl==3.0.5
  Caused by: Couldn't parse metadata of openpyxl-3.0.5-py2.py3-none-any.whl from https://files.pythonhosted.org/packages/5c/90/61f83be1c335a9b69fa773784a785d9de95c7561d1661918796fd1cba3d2/openpyxl-3.0.5-py2.py3-none-any.whl
  Caused by: Failed to parse version: Unexpected end of version specifier, expected operator:
>=3.6, 
```

```
uv --version
uv 0.1.0
```

---

_Label `bug` added by @zanieb on 2024-02-15 21:40_

---

_Comment by @zanieb on 2024-02-15 21:41_

Thanks for the report! We'll add this to our mistake-tolerant parser.

---

_Assigned to @zanieb by @zanieb on 2024-02-15 21:41_

---

_Renamed from "Failed to parse version with 'coma', `>=3.6,` at end" to "Failed to parse version with 'comma', `>=3.6,` at end" by @onjin on 2024-02-15 21:43_

---

_Comment by @zanieb on 2024-02-15 21:53_

Interesting we have support for this 

https://github.com/astral-sh/uv/blob/b1edecdf1f048941305ad43a3cc727e4c2b2c9b5/crates/pypi-types/src/lenient_requirement.rs#L181-L186

We'll need to see why it's not being used


---

_Comment by @onjin on 2024-02-15 23:07_

It comes from `"requires-python": ">=3.6,",` entry

---

_Comment by @zanieb on 2024-02-15 23:08_

Thanks we might treat those differently!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-16 01:49_

---

_Unassigned @zanieb by @charliermarsh on 2024-02-16 01:49_

---

_Comment by @charliermarsh on 2024-02-16 01:49_

I can take this one real quick.

---

_Closed by @charliermarsh on 2024-02-16 02:08_

---
