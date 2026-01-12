```yaml
number: 4031
title: "Clarify role of `--system` flag in README"
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/docs-discovery
created_at: 2024-06-04T23:53:29Z
updated_at: 2024-06-10T16:19:42Z
url: https://github.com/astral-sh/uv/pull/4031
synced_at: 2026-01-12T16:05:59Z
```

# Clarify role of `--system` flag in README

---

_@zanieb_

For uv>=0.2.0 behavior

Closes #3951 

---

_Label `documentation` added by @zanieb on 2024-06-04 23:53_

---

_Review comment by @dpoznik on `README.md`:201 on 2024-06-06 15:07_

```suggestion
The `--system` flag is also used to opt in to mutating system environments. For example, the
the `--python` argument can be used to request a Python version (e.g., `--python 3.12`), and uv will
search for an interpreter that meets the request. If uv finds a system interpreter (e.g., `/usr/lib/python3.12`),
then the `--system` flag is required to allow modification of this non-virtual Python environment.
Without the `--system` flag, uv will ignore any interpreters that are not in virtual environments.
Conversely, when the `--system` flag is provided, uv will ignore any interpreters that *are*
in virtual environments.
```

---

_@dpoznik approved on 2024-06-06 15:08_

This reads well and clarifies a few points. Thanks!
Some minor typographical suggestions below; feel free to ignore.


---

_Marked ready for review by @zanieb on 2024-06-06 23:54_

---

_Review requested from @charliermarsh by @zanieb on 2024-06-10 14:29_

---

_Review comment by @charliermarsh on `README.md`:191 on 2024-06-10 15:44_

Nit: "but note that"

---

_Review comment by @charliermarsh on `README.md`:191 on 2024-06-10 15:45_

Could "executables on the `PATH` that execute in a virtual environment." be something like... "executables that are linked to virtual environments"?

---

_Review comment by @charliermarsh on `README.md`:185 on 2024-06-10 15:46_

This is confusing because `--system` is _required_ for this now, right?

---

_@charliermarsh reviewed on 2024-06-10 15:46_

---

_@charliermarsh approved on 2024-06-10 15:48_

---

_@zanieb reviewed on 2024-06-10 16:03_

---

_Review comment by @zanieb on `README.md`:185 on 2024-06-10 16:03_

No, if it's explicit (i.e. that path to the executable or its directory) we do not require the `--system` flag.

---

_@zanieb reviewed on 2024-06-10 16:04_

---

_Review comment by @zanieb on `README.md`:191 on 2024-06-10 16:04_

```suggestion
but note that executables that are linked to virtual environments will be skipped.
```

---

_Merged by @zanieb on 2024-06-10 16:19_

---

_Closed by @zanieb on 2024-06-10 16:19_

---

_Branch deleted on 2024-06-10 16:19_

---
