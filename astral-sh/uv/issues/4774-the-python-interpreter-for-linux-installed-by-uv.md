```yaml
number: 4774
title: "The python interpreter for linux installed by `uv toolchain` is debug built."
type: issue
state: closed
author: Di-Is
labels:
  - bug
  - preview
assignees: []
created_at: 2024-07-03T14:41:09Z
updated_at: 2024-07-03T15:58:06Z
url: https://github.com/astral-sh/uv/issues/4774
synced_at: 2026-01-12T15:58:52Z
```

# The python interpreter for linux installed by `uv toolchain` is debug built.

---

_@Di-Is_

The python interpreter for linux installed by `uv toolchain` seems to be built in debug mode.
From a performance standpoint, it is better to use optimized binaries like pgo+lto instead.

#### Check command

```bash
# check version
uv version
uv 0.2.21

# install python
uv toolchain install 3.12.3

# check build type
uv run python -c "import sysconfig;print(sysconfig.get_config_var('Py_DEBUG'))"
1
```

---

_Comment by @zanieb on 2024-07-03 14:52_

Hm we should be preferring the optimized one, let me check what's going on here.

---

_Assigned to @zanieb by @zanieb on 2024-07-03 15:20_

---

_Label `bug` added by @zanieb on 2024-07-03 15:20_

---

_Label `preview` added by @zanieb on 2024-07-03 15:20_

---

_Closed by @zanieb on 2024-07-03 15:58_

---

_Closed by @zanieb on 2024-07-03 15:58_

---
