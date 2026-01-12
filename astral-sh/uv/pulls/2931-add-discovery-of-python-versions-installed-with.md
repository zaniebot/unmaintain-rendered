```yaml
number: 2931
title: "Add discovery of Python versions installed with `uv-toolchain`"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: zb/uv-toolchain
head: zb/uv-toolchain-discovery
created_at: 2024-04-09T14:08:10Z
updated_at: 2024-04-10T14:01:03Z
url: https://github.com/astral-sh/uv/pull/2931
synced_at: 2026-01-12T16:05:19Z
```

# Add discovery of Python versions installed with `uv-toolchain`

---

_@zanieb_

Completes #2842 by fixing Windows tests.

Replaces our current bootstrapped Python version loading in tests with some naive toolchain Python version discovery.

There are no user facing changes here and there should be no change in test experience. I confirmed the test suite only fails on the following cases when the toolchain path is not populated:

```
uv::pip_compile_scenarios python_patch_override_no_patch
uv::pip_compile_scenarios python_patch_override_patch_compatible
uv::pip_install_scenarios python_greater_than_current_patch
uv::venv create_venv_python_patch
```


---

_Label `internal` added by @zanieb on 2024-04-09 14:08_

---

_Marked ready for review by @zanieb on 2024-04-09 17:27_

---

_@zanieb reviewed on 2024-04-09 17:28_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/interpreter.rs`:323 on 2024-04-09 17:28_

Inverse of `PythonVersion::is_satisfied_by(... Interpreter)` due to the dependency reversal

---

_Review requested from @charliermarsh by @zanieb on 2024-04-09 17:33_

---

_Review requested from @konstin by @zanieb on 2024-04-09 17:33_

---

_@charliermarsh reviewed on 2024-04-10 02:39_

---

_Review comment by @charliermarsh on `crates/uv-toolchain/src/find.rs`:88 on 2024-04-10 02:39_

Maybe just do this once, instead of in both conditions? Since it _can_ allocate.

---

_@charliermarsh reviewed on 2024-04-10 02:40_

---

_Review comment by @charliermarsh on `crates/uv-toolchain/src/find.rs`:27 on 2024-04-10 02:40_

Can you add a Rustdoc for this? It's the path to what?

---

_@charliermarsh approved on 2024-04-10 02:41_

---

_Review comment by @konstin on `crates/uv-interpreter/src/interpreter.rs`:319 on 2024-04-10 11:10_

```suggestion
    /// Check if the interpreter matches the given Python version.
```

---

_Review comment by @konstin on `crates/uv-toolchain/src/find.rs`:9 on 2024-04-10 11:11_

```suggestion
/// The directory where Python toolchains we install are stored.
```

---

_@konstin approved on 2024-04-10 11:18_

---

_Merged by @zanieb on 2024-04-10 14:00_

---

_Closed by @zanieb on 2024-04-10 14:00_

---

_Branch deleted on 2024-04-10 14:00_

---

_Comment by @zanieb on 2024-04-10 14:01_

Merged into #2842 

---
