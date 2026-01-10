```yaml
number: 16824
title: "Validate URL wheel tags against `Requires-Python` and required environments"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/req
created_at: 2025-11-23T21:08:49Z
updated_at: 2025-11-26T01:06:00Z
url: https://github.com/astral-sh/uv/pull/16824
synced_at: 2026-01-10T05:58:11Z
```

# Validate URL wheel tags against `Requires-Python` and required environments

---

_Pull request opened by @charliermarsh on 2025-11-23 21:08_

## Summary

Closes #16818.


---

_Label `bug` added by @charliermarsh on 2025-11-23 21:08_

---

_Review requested from @konstin by @charliermarsh on 2025-11-23 21:08_

---

_Marked ready for review by @charliermarsh on 2025-11-23 21:08_

---

_@charliermarsh reviewed on 2025-11-23 21:09_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/lock.rs`:32247 on 2025-11-23 21:09_

Today, this resolution passes and the wheel is just omitted from the lockfile.

---

_@charliermarsh reviewed on 2025-11-23 21:09_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/lock.rs`:32279 on 2025-11-23 21:09_

Same here -- this passes too.

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:1202 on 2025-11-24 12:51_

Here, we compute through which marker paths the package is currently reachable. Doesn't that mean we can miss an incompatible package if the dependency with a wider marker only appears later?

---

_@konstin reviewed on 2025-11-24 12:52_

One question about the resolver order, otherwise looks good.

---

_@charliermarsh reviewed on 2025-11-24 14:08_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:1202 on 2025-11-24 14:08_

Hmm, maybe? But I think this is the same strategy as in the `choose_version_registry` path`.

---

_@konstin reviewed on 2025-11-24 14:59_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:1202 on 2025-11-24 14:59_

It looks like we're getting saved by pseudo-packages having their version selection, with this PR this fails as it should:

```toml
[project]
name = "test-toml"
version = "0.1.0"
requires-python = ">=3.14"
dependencies = [
    'numpy; sys_platform == "win32"',
    'pandas',
]

[tool.uv]
required-environments = ['sys_platform == "linux"', 'sys_platform == "win32"']

[tool.uv.sources]
numpy = { url = "https://files.pythonhosted.org/packages/a3/2e/235b4d96619931192c91660805e5e49242389742a7a82c27665021db690c/numpy-2.3.5-cp314-cp314-win_amd64.whl" }
```

---

_@konstin approved on 2025-11-24 14:59_

---

_Merged by @charliermarsh on 2025-11-26 01:05_

---

_Closed by @charliermarsh on 2025-11-26 01:05_

---

_Branch deleted on 2025-11-26 01:06_

---
