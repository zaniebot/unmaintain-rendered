```yaml
number: 436
title: Support path dependencies
type: issue
state: closed
author: konstin
labels:
  - enhancement
assignees: []
created_at: 2023-11-16T13:24:30Z
updated_at: 2023-11-21T11:49:44Z
url: https://github.com/astral-sh/uv/issues/436
synced_at: 2026-01-10T05:40:31Z
```

# Support path dependencies

---

_Issue opened by @konstin on 2023-11-16 13:24_

We should support path dependencies. They come in two flavors: One is "source dist flavored", where we just treat them like an unpacked source distribution or git checkout and build them with PEP 517 or setup.py. The other is "editable flavored" using [PEP 660](https://peps.python.org/pep-0660/), supported by an additional `build_editable` hook. In requirements.txt the latter form is requested by `-e`. We have to investigate the correct behavior for pyproject.toml requirements. This is an essential part for workspace support.

---

_Label `enhancement` added by @charliermarsh on 2023-11-16 17:43_

---

_Added to milestone `Feature complete` by @charliermarsh on 2023-11-16 17:43_

---

_Comment by @charliermarsh on 2023-11-17 02:11_

I can do path dependencies (without editable) pretty easily, I think.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-17 02:33_

---

_Closed by @charliermarsh on 2023-11-21 11:49_

---
