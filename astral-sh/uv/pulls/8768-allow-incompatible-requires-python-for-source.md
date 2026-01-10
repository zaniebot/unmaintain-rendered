```yaml
number: 8768
title: "Allow incompatible `requires-python` for source distributions with static metadata"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/later
created_at: 2024-11-02T21:31:55Z
updated_at: 2024-11-03T19:03:56Z
url: https://github.com/astral-sh/uv/pull/8768
synced_at: 2026-01-10T11:59:59Z
```

# Allow incompatible `requires-python` for source distributions with static metadata

---

_Pull request opened by @charliermarsh on 2024-11-02 21:31_

## Summary

At present, when we have a Python requirement and we see a wheel, we verify that the Python requirement is compatible with the wheel. For source distributions, though, we verify that both the Python requirement _and_ the currently-installed version are compatible, because we assume that we'll need to build the source distribution in order to get metadata. However, we can often extract source distribution metadata _without_ building (e.g., if there's a `pyproject.toml` with no dynamic keys).

This PR thus modifies the source distribution handling to defer that incompatibility ("We couldn't get metadata for this project, because it has no static metadata and requires a higher Python version to run / build") until we actually try to build the package. As a result, you can now resolve source distribution-only packages using Python versions below their `requires-python`, as long as they include static metadata.

Closes https://github.com/astral-sh/uv/issues/8767.


---

_Review requested from @zanieb by @charliermarsh on 2024-11-03 02:44_

---

_Review requested from @konstin by @charliermarsh on 2024-11-03 02:44_

---

_Label `bug` added by @charliermarsh on 2024-11-03 02:46_

---

_Marked ready for review by @charliermarsh on 2024-11-03 02:46_

---

_Review comment by @konstin on `crates/uv/tests/it/pip_compile.rs`:12677 on 2024-11-03 17:14_

Should we add a `--no-build` for extra checks?

---

_@konstin approved on 2024-11-03 17:14_

Good find

---

_@charliermarsh reviewed on 2024-11-03 18:42_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/pip_compile.rs`:12677 on 2024-11-03 18:42_

It actually doesn't work with `--no-build` (we fully fail, since we don't even consider sdists). Unsure whether it should... I could go either way on it.

---

_@konstin reviewed on 2024-11-03 19:01_

---

_Review comment by @konstin on `crates/uv/tests/it/pip_compile.rs`:12677 on 2024-11-03 19:01_

That is a tough one: Should you be able to resolve with `--no-build`, when we already know that we can't install with `--no-build`?

Can you open an to track that separately?

---

_@charliermarsh reviewed on 2024-11-03 19:03_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/pip_compile.rs`:12677 on 2024-11-03 19:03_

Yeah.

---

_Merged by @charliermarsh on 2024-11-03 19:03_

---

_Closed by @charliermarsh on 2024-11-03 19:03_

---

_Branch deleted on 2024-11-03 19:03_

---
