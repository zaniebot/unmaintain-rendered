---
number: 8767
title: "In resolver, allow source distributions with static metadata, but no `requires-python` compatibility"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2024-11-02T21:13:19Z
updated_at: 2024-11-03T19:03:57Z
url: https://github.com/astral-sh/uv/issues/8767
synced_at: 2026-01-07T13:12:18-06:00
---

# In resolver, allow source distributions with static metadata, but no `requires-python` compatibility

---

_Issue opened by @charliermarsh on 2024-11-02 21:13_

The following fails to resolve under `--python 3.11`, because `interpreters-pep-734` has no wheels, and it requires at least Python 3.13, ostensibly to get the metadata -- however, the package has static metadata in the `pyproject.toml`, so we should actually be able to resolve it without issue:

```toml
[project]
requires-python = ">=3.11"

[project.optional-dependencies]
sub = [
    "interpreters-pep-734>=0.4.1; python_version ~= '3.13'",
]
```


---

_Label `bug` added by @charliermarsh on 2024-11-02 21:13_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-02 21:13_

---

_Referenced in [astral-sh/uv#8768](../../astral-sh/uv/pulls/8768.md) on 2024-11-02 21:31_

---

_Referenced in [ericsnowcurrently/interpreters#16](../../ericsnowcurrently/interpreters/issues/16.md) on 2024-11-02 23:40_

---

_Referenced in [TkTech/chancy#9](../../TkTech/chancy/pulls/9.md) on 2024-11-03 01:27_

---

_Closed by @charliermarsh on 2024-11-03 19:03_

---

_Closed by @charliermarsh on 2024-11-03 19:03_

---
