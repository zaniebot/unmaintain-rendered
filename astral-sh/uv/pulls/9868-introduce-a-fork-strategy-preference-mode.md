```yaml
number: 9868
title: "Introduce a `--fork-strategy` preference mode"
type: pull_request
state: merged
author: charliermarsh
labels:
  - configuration
assignees: []
merged: true
base: main
head: charlie/max-requires-python
created_at: 2024-12-13T14:13:00Z
updated_at: 2024-12-13T23:46:23Z
url: https://github.com/astral-sh/uv/pull/9868
synced_at: 2026-01-10T12:00:01Z
```

# Introduce a `--fork-strategy` preference mode

---

_Pull request opened by @charliermarsh on 2024-12-13 14:13_

## Summary

This PR makes the behavior in https://github.com/astral-sh/uv/pull/9827 the default: we try to select the latest supported package version for each supported Python version, but we still optimize for choosing fewer versions when stratifying by platform.

However, you can opt out with `--fork-strategy fewest`.

Closes https://github.com/astral-sh/uv/issues/7190.


---

_Review comment by @T-256 on `crates/uv/tests/it/lock.rs`:17366 on 2024-12-13 17:12_

Could these simplified as `"python_full_version >= '3.10'"`? like what's done on below lines:
```toml
[[package]]
name = "project"
...
dependencies = [
    { name = "numpy", version = "1.24.4", source = { registry = "https://pypi.org/simple" }, marker = "python_full_version < '3.9'" },
    { name = "numpy", version = "2.0.2", source = { registry = "https://pypi.org/simple" }, marker = "python_full_version == '3.9.*'" },
    { name = "numpy", version = "2.1.2", source = { registry = "https://pypi.org/simple" }, marker = "python_full_version >= '3.10'" },
]
```

---

_Review comment by @T-256 on `crates/uv/tests/it/lock.rs`:17432 on 2024-12-13 17:13_

⬆️ 

---

_@T-256 reviewed on 2024-12-13 17:15_

---

_Renamed from "Introduce a `--multi-version` preference mode" to "Introduce a `--fork-strategy` preference mode" by @charliermarsh on 2024-12-13 20:49_

---

_@charliermarsh reviewed on 2024-12-13 20:51_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/lock.rs`:17366 on 2024-12-13 20:51_

They need to be disjoint, so I believe this is the best we can do.

---

_Label `configuration` added by @charliermarsh on 2024-12-13 20:53_

---

_Merged by @charliermarsh on 2024-12-13 21:05_

---

_Closed by @charliermarsh on 2024-12-13 21:05_

---

_Branch deleted on 2024-12-13 21:05_

---

_@ryanhiebert reviewed on 2024-12-13 23:07_

---

_Review comment by @ryanhiebert on `docs/reference/settings.md`:780 on 2024-12-13 23:07_

I'm seeing several references suggesting that `requires-python` is the default, but this says `fewest`.

---

_@charliermarsh reviewed on 2024-12-13 23:25_

---

_Review comment by @charliermarsh on `docs/reference/settings.md`:780 on 2024-12-13 23:25_

Thanks, that’s a mistake. Requires-Python is the default.

---

_Review comment by @ryanhiebert on `docs/reference/settings.md`:780 on 2024-12-13 23:46_

Thanks! I'm happy for that to be the default!

---

_@ryanhiebert reviewed on 2024-12-13 23:46_

---
