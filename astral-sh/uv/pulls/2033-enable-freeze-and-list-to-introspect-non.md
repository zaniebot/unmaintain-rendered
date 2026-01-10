```yaml
number: 2033
title: "Enable `freeze` and `list` to introspect non-virtualenv Pythons"
type: pull_request
state: merged
author: charliermarsh
labels:
  - compatibility
assignees: []
merged: true
base: main
head: charlie/freeze
created_at: 2024-02-28T04:06:55Z
updated_at: 2024-02-28T15:00:02Z
url: https://github.com/astral-sh/uv/pull/2033
synced_at: 2026-01-10T14:54:43Z
```

# Enable `freeze` and `list` to introspect non-virtualenv Pythons

---

_Pull request opened by @charliermarsh on 2024-02-28 04:06_

## Summary

Now that we have the ability to introspect the installed packages for arbitrary Pythons, we can allow `pip freeze` and `pip list` to fall back to the "default" Python, if no virtualenv is present.

Closes https://github.com/astral-sh/uv/issues/2005.


---

_Renamed from "Enable `freeze` and `list` to enumerate default Python installs" to "Enable `freeze` and `list` to introspect non-virtualenv Pythons" by @charliermarsh on 2024-02-28 04:07_

---

_Label `compatibility` added by @charliermarsh on 2024-02-28 04:07_

---

_@charliermarsh reviewed on 2024-02-28 04:08_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip_freeze.rs`:34 on 2024-02-28 04:08_

This does mean that if a virtualenv doesn't exist, we "silently" fall back to the default Python; and you'll never see the "No virtualenv exists" error, only the error you might get if the fallback fails.

It kind of makes me want to change this to user-facing logging?

```rust
debug!(
    "Using Python {} environment at {}",
    venv.interpreter().python_version(),
    venv.python_executable().normalized_display().cyan()
);
```

---

_Review requested from @zanieb by @charliermarsh on 2024-02-28 04:08_

---

_Review requested from @konstin by @charliermarsh on 2024-02-28 04:08_

---

_Review comment by @konstin on `crates/uv/src/commands/pip_freeze.rs`:34 on 2024-02-28 09:31_

:+1:

---

_Review comment by @konstin on `crates/uv/src/main.rs`:737 on 2024-02-28 09:31_

```suggestion
    /// environment `.venv` located in the current working directory or any parent directory, falling back
```

---

_Review comment by @konstin on `crates/uv/src/main.rs`:742 on 2024-02-28 09:32_

Patch versions are supported 

---

_@konstin approved on 2024-02-28 09:33_

---

_Merged by @charliermarsh on 2024-02-28 15:00_

---

_Closed by @charliermarsh on 2024-02-28 15:00_

---

_Branch deleted on 2024-02-28 15:00_

---
