```yaml
number: 2573
title: "Follow-up fixes for `__main__.py`"
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
base: main
head: charlie/main
created_at: 2023-02-04T21:32:24Z
updated_at: 2023-02-04T21:33:50Z
url: https://github.com/astral-sh/ruff/pull/2573
synced_at: 2026-01-12T15:55:08Z
```

# Follow-up fixes for `__main__.py`

---

_@charliermarsh_

See comments in #2563.

---

_Comment by @charliermarsh on 2023-02-04 21:32_

\cc @andersk 

---

_Renamed from "Follow-up fixes for __main__.py" to "Follow-up fixes for `__main__.py`" by @charliermarsh on 2023-02-04 21:32_

---

_@charliermarsh reviewed on 2023-02-04 21:33_

---

_Review comment by @charliermarsh on `python/ruff/__main__.py`:21 on 2023-02-04 21:33_

We can also try falling back to `Path(sysconfig.get_config_var("userbase")) / "bin" / "ruff"`, it shouldn't hurt, it just might be wrong on Windows.

---

_Comment by @charliermarsh on 2023-02-04 21:33_

Oops -- closing in favor of #2574.

---

_Closed by @charliermarsh on 2023-02-04 21:33_

---

_Branch deleted on 2023-02-04 21:33_

---
