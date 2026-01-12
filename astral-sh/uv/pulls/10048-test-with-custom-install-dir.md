```yaml
number: 10048
title: Test with custom install dir
type: pull_request
state: merged
author: 1hakusai1
labels:
  - documentation
  - testing
assignees: []
merged: true
base: main
head: test_with_custom_install_dir
created_at: 2024-12-20T06:52:03Z
updated_at: 2025-03-03T03:58:55Z
url: https://github.com/astral-sh/uv/pull/10048
synced_at: 2026-01-12T16:09:06Z
```

# Test with custom install dir

---

_@1hakusai1_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Testing with `UV_PYTHON_INSTALL_DIR` environment variable has some problems. This PR fix them.

- `UV_PYTHON_INSTALL_DIR` must be an absolute path.
  - Cargo tries to find Python executables from each crates in test. If it is relative path, cargo searches in different directories for each tests.
- Skip the test asserting help messages.
  - Clap shows the current value of the environment variables. If `UV_PYTHON_INSTALL_DIR` is set, the test fails.

## Test Plan

<!-- How was it tested? -->

All tests pass with `UV_PYTHON_INSTALL_DIR=/path/to/my/home/uv/target/testpython`.


---

_Comment by @1hakusai1 on 2024-12-20 07:17_

One test fails, but it seems not to be related to this change.

---

_@samypr100 reviewed on 2024-12-21 02:30_

---

_Review comment by @samypr100 on `crates/uv/tests/it/help.rs`:458 on 2024-12-21 02:30_

Note, you can use instead

`uv_snapshot!(context.filters(), context.help().env_remove(EnvVars::UV_PYTHON_INSTALL_DIR).arg("python").arg("install"), @r###"`


---

_Label `documentation` added by @samypr100 on 2024-12-21 02:30_

---

_Review requested from @samypr100 by @1hakusai1 on 2025-02-20 13:18_

---

_Merged by @charliermarsh on 2025-03-03 03:58_

---

_Closed by @charliermarsh on 2025-03-03 03:58_

---

_Label `testing` added by @charliermarsh on 2025-03-03 03:58_

---
