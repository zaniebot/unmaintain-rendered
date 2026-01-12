```yaml
number: 3069
title: Add support for .tar.bz2 source distributions
type: pull_request
state: merged
author: sergeykolosov
labels: []
assignees: []
merged: true
base: main
head: archive-type-bz2
created_at: 2024-04-16T17:57:48Z
updated_at: 2024-04-27T19:02:24Z
url: https://github.com/astral-sh/uv/pull/3069
synced_at: 2026-01-12T16:05:24Z
```

# Add support for .tar.bz2 source distributions

---

_@sergeykolosov_

## Summary

Source distributions in the .tar.bz2 format are still relatively common within the existing code-bases, namely, the most common examples are the Twisted source distributions up to the version 20.3.0. As quite so often the ability to upgrade Twisted to a more recent version is not available for a given project, we add the support for .tar.bz2 here to still allow `uv` to be a drop-in replacement for `pip` in these projects.

## Test Plan

The feature was tested both by adding the corresponding test coverage, and by directly installing a package of interest under a Python version that doesn't have the corresponding wheel:

```sh
cargo run venv -p python3.8
cargo run pip install Twisted==20.3.0 --no-cache
```

The `--no-cache` argument in the example above serves the purpose of cleaning the cached information regarding the unsatisfiability of the requirements, as it may have been cached during some previous attempt to install this package by `uv` version that didn't implement this feature yet.

---

_Renamed from "Added support for .tar.bz2 source distributions" to "Add support for .tar.bz2 source distributions" by @sergeykolosov on 2024-04-16 17:58_

---

_@charliermarsh approved on 2024-04-16 17:59_

Thanks, this looks reasonable to me.

---

_@charliermarsh reviewed on 2024-04-16 18:00_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_install.rs`:314 on 2024-04-16 18:00_

Can you change this to use `--no-binary twisted`, just to be sure that we aren't using the wheel? Also, can you make this a `pip sync` test instead to avoid the extra work downloading those other packages?

---

_@sergeykolosov reviewed on 2024-04-16 18:01_

---

_Review comment by @sergeykolosov on `crates/uv/tests/pip_install.rs`:314 on 2024-04-16 18:01_

Sure, working on it.

---

_@sergeykolosov reviewed on 2024-04-16 18:28_

---

_Review comment by @sergeykolosov on `crates/uv/tests/pip_install.rs`:314 on 2024-04-16 18:28_

Pushed the changes.

---

_@charliermarsh reviewed on 2024-04-16 18:31_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_install.rs`:314 on 2024-04-16 18:31_

Thanks!

---

_Merged by @charliermarsh on 2024-04-16 18:34_

---

_Closed by @charliermarsh on 2024-04-16 18:34_

---

_Branch deleted on 2024-04-27 19:02_

---
