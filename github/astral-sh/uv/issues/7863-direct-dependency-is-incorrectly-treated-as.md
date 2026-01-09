---
number: 7863
title: Direct dependency is incorrectly treated as indirect dependency
type: issue
state: closed
author: konstin
labels:
  - bug
  - good first issue
assignees: []
created_at: 2024-10-02T09:46:19Z
updated_at: 2024-12-29T01:32:46Z
url: https://github.com/astral-sh/uv/issues/7863
synced_at: 2026-01-07T13:12:17-06:00
---

# Direct dependency is incorrectly treated as indirect dependency

---

_Issue opened by @konstin on 2024-10-02 09:46_

We incorrectly treat numpy as a transitive instead of a direct dependency, which means we don't emit the correct diagnostic about a missing lower bound:

```
[project]
name = "dummy"
version = "0.1.0"
description = "Add your description here"
requires-python = ">=3.9"
dependencies = [
  "numpy",
]
```

Relevant log line from `RUST_LOG=uv=debug uv lock`:

```
DEBUG Adding transitive dependency for dummy==0.1.0: numpy*
```

We need to change this check to consider workspace packages:

https://github.com/astral-sh/uv/blob/30e11c6debf2ceb53710fdfbafcb87c6d478aa1b/crates/uv-resolver/src/resolver/mod.rs#L2163

---

_Label `bug` added by @konstin on 2024-10-02 09:46_

---

_Label `good first issue` added by @konstin on 2024-10-02 09:46_

---

_Comment by @mstepan on 2024-10-03 11:27_

I was trying to reproduce this issue using posted `pyproject.toml` file but seems it's working as expected:

```
uv/target/debug/uv pip compile example/pyproject.toml -o example/requirements.txt
...

DEBUG Adding direct dependency: numpy*
INFO add_decision: example @ 0a0.dev0 
```

---

_Comment by @charliermarsh on 2024-10-03 11:51_

Try using `uv lock` instead of `uv pip compile`.

---

_Comment by @Aditya-PS-05 on 2024-10-16 13:32_

@konstin 
I am solving it and setting this type of diagnostic message,

![Screenshot from 2024-10-16 18-39-54](https://github.com/user-attachments/assets/6a7a486f-f2bc-4071-bfaf-bcabe45777eb)

Is this the required behavior or somethingelse

---

_Comment by @konstin on 2024-10-16 13:45_

We should treat numpy as a direct dependency here, which means we should show the `The direct dependency `{package}` is unpinned.` message [here](https://github.com/astral-sh/uv/blob/2153c6ac0d692b9eccedbd7567087907667645fb/crates/uv-resolver/src/resolver/mod.rs#L2254) for numpy: We should have an additional check that the `for_package` isn't a workspace member.

---

_Comment by @Aditya-PS-05 on 2024-10-16 15:36_

How should I check if it's a workspace member? Should I need to explicitly define that these are the workspace members or something else?

---

_Comment by @konstin on 2024-10-16 15:38_

`ResolverState::workspace_members` has this information

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-27 16:09_

---

_Referenced in [astral-sh/uv#10197](../../astral-sh/uv/pulls/10197.md) on 2024-12-27 16:09_

---

_Closed by @charliermarsh on 2024-12-29 01:32_

---
