---
number: 5814
title: uv add --dev does not support virtual workspace roots
type: issue
state: closed
author: bluss
labels:
  - bug
  - projects
  - preview
assignees: []
created_at: 2024-08-06T14:24:17Z
updated_at: 2024-08-06T18:06:05Z
url: https://github.com/astral-sh/uv/issues/5814
synced_at: 2026-01-10T01:23:52Z
---

# uv add --dev does not support virtual workspace roots

---

_Issue opened by @bluss on 2024-08-06 14:24_

I'm not sure this should be a feature, but maybe.

Reproduce:

```shell
uv init --virtual myroot
cd myroot
uv add --dev numpy
```

Actual behaviour

```
error: No `project` table found in: `/home/user/myroot/pyproject.toml
```

Expected - added to [tools.uv.dev-dependencies].

The virtual workspace root already can do: dependencies syncing and setting up the virtualenv, using `uv sync`. Running dev-deps as tools using `uv run`. It can do these right now without any projects as members.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-06 14:39_

---

_Label `bug` added by @zanieb on 2024-08-06 14:39_

---

_Label `projects` added by @zanieb on 2024-08-06 14:39_

---

_Label `preview` added by @zanieb on 2024-08-06 14:39_

---

_Referenced in [astral-sh/uv#5821](../../astral-sh/uv/pulls/5821.md) on 2024-08-06 17:48_

---

_Closed by @charliermarsh on 2024-08-06 18:06_

---
