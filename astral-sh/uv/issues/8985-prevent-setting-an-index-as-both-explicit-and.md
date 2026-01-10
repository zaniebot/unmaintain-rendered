```yaml
number: 8985
title: Prevent setting an index as both explicit and default
type: issue
state: closed
author: mkniewallner
labels:
  - bug
assignees: []
created_at: 2024-11-10T10:51:09Z
updated_at: 2024-11-10T18:05:41Z
url: https://github.com/astral-sh/uv/issues/8985
synced_at: 2026-01-10T04:36:20Z
```

# Prevent setting an index as both explicit and default

---

_Issue opened by @mkniewallner on 2024-11-10 10:51_

Unless I'm missing a use case, it seems to me that `default = true` won't actually do anything when set on an index that also sets `explicit = true`.

Consider for instance:

```toml
[project]
name = "foo"
version = "0.0.1"
dependencies = ["mypy==1.13.0"]

[[tool.uv.index]]
name = "test-pypi"
url = "https://test.pypi.org/simple"
explicit = true
default = true
```

`default = true` allows disabling default PyPi index, but in that specific case, it won't, which is expected, since the index is an explicit one, so `mypy` will correctly be resolved from public PyPi.

If there really is no use case for having both `explicit = true` and `default = true` for an index, should uv error out, to better convey that this is not a real use case?

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-10 15:50_

---

_Comment by @charliermarsh on 2024-11-10 15:52_

I think this probably should be allowed.

---

_Label `bug` added by @charliermarsh on 2024-11-10 16:00_

---

_Closed by @charliermarsh on 2024-11-10 18:05_

---

_Closed by @charliermarsh on 2024-11-10 18:05_

---
