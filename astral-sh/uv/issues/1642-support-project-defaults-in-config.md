---
number: 1642
title: Support project defaults in config
type: issue
state: closed
author: rik
labels:
  - enhancement
  - configuration
assignees: []
created_at: 2024-02-18T12:10:35Z
updated_at: 2024-05-23T22:44:37Z
url: https://github.com/astral-sh/uv/issues/1642
synced_at: 2026-01-10T01:23:08Z
---

# Support project defaults in config

---

_Issue opened by @rik on 2024-02-18 12:10_

pip-tools allows defining [default values for any command-line flag](https://pip-tools.readthedocs.io/en/stable/#configuration). I haven't found a way to do so for uv.

I use it to avoid passing `--generate-hashes`.

---

_Label `configuration` added by @zanieb on 2024-02-18 21:13_

---

_Label `enhancement` added by @zanieb on 2024-02-18 21:13_

---

_Comment by @charliermarsh on 2024-05-22 13:28_

We now support `uv.toml` which allows this.

---

_Closed by @charliermarsh on 2024-05-22 13:28_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-22 13:28_

---

_Comment by @rik on 2024-05-23 22:44_

Thanks! And it also works within `pyproject.toml` which is great.

---
