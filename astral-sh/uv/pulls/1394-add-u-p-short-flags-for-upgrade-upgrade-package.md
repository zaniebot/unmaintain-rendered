```yaml
number: 1394
title: "Add `-U`/`-P` short flags for `--upgrade`/`--upgrade-package`"
type: pull_request
state: merged
author: zanieb
labels:
  - compatibility
assignees: []
merged: true
base: main
head: zb/pip-U
created_at: 2024-02-16T00:01:12Z
updated_at: 2024-02-16T01:34:20Z
url: https://github.com/astral-sh/uv/pull/1394
synced_at: 2026-01-12T16:04:37Z
```

# Add `-U`/`-P` short flags for `--upgrade`/`--upgrade-package`

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/1340

---

_Renamed from "Add `-U` short flag for `--upgrade`" to "Add `-U`/`-P` short flags for `--upgrade`/`--upgrade-package`" by @zanieb on 2024-02-16 00:02_

---

_Label `enhancement` added by @zanieb on 2024-02-16 00:02_

---

_Label `enhancement` removed by @zanieb on 2024-02-16 00:02_

---

_Label `compatibility` added by @zanieb on 2024-02-16 00:02_

---

_@charliermarsh reviewed on 2024-02-16 01:22_

---

_Review comment by @charliermarsh on `crates/uv/src/main.rs`:264 on 2024-02-16 01:22_

I find `-P` unexpected, but I see that this is what pip-tools does.

---

_@charliermarsh approved on 2024-02-16 01:22_

---

_@zanieb reviewed on 2024-02-16 01:25_

---

_Review comment by @zanieb on `crates/uv/src/main.rs`:264 on 2024-02-16 01:25_

Me too 🤷‍♀️ seems okay for this interface

---

_Merged by @zanieb on 2024-02-16 01:34_

---

_Closed by @zanieb on 2024-02-16 01:34_

---

_Branch deleted on 2024-02-16 01:34_

---
