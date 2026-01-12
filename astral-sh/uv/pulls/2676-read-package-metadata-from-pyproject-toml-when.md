```yaml
number: 2676
title: "Read package metadata from `pyproject.toml` when statically defined"
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/pyproject
created_at: 2024-03-26T20:23:11Z
updated_at: 2024-03-27T14:34:19Z
url: https://github.com/astral-sh/uv/pull/2676
synced_at: 2026-01-12T16:05:10Z
```

# Read package metadata from `pyproject.toml` when statically defined

---

_@charliermarsh_

## Summary

Now that we're resolving metadata more aggressively for local sources, it's worth doing this. We now pull metadata from the `pyproject.toml` directly if it's statically-defined.

Closes https://github.com/astral-sh/uv/issues/2629.


---

_Label `performance` added by @charliermarsh on 2024-03-26 20:23_

---

_Marked ready for review by @charliermarsh on 2024-03-26 20:57_

---

_Review requested from @zanieb by @charliermarsh on 2024-03-26 21:05_

---

_Review requested from @konstin by @charliermarsh on 2024-03-26 21:05_

---

_@charliermarsh reviewed on 2024-03-26 21:05_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_install.rs`:902 on 2024-03-26 21:05_

This is correct, because we no longer download `flit-core` during the resolution phase.

---

_Review comment by @konstin on `crates/pypi-types/src/metadata.rs`:278 on 2024-03-27 09:19_

```suggestion
/// relevant for dependency resolution.
/// See <https://packaging.python.org/en/latest/specifications/pyproject-toml>.
```

---

_Review comment by @konstin on `crates/pypi-types/src/metadata.rs`:447 on 2024-03-27 09:21_

That is even an invalid pyproject.toml ("The keys which are required but may be specified either statically or listed as dynamic are: version")

---

_Review comment by @konstin on `crates/pypi-types/src/metadata.rs`:470 on 2024-03-27 09:22_

I'm pretty sure most users don't get that eliding the `dependencies` key means declaring that you have no deps, but that's the spec.

---

_@konstin approved on 2024-03-27 09:25_

---

_Merged by @charliermarsh on 2024-03-27 14:34_

---

_Closed by @charliermarsh on 2024-03-27 14:34_

---

_Branch deleted on 2024-03-27 14:34_

---
