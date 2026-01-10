```yaml
number: 3318
title: "Add `UV_NO_BUILD_ISOLATION` as environment variable"
type: pull_request
state: merged
author: flyaroundme
labels:
  - configuration
assignees: []
merged: true
base: main
head: no-build-isolation-env-var
created_at: 2024-04-30T03:57:22Z
updated_at: 2024-04-30T13:29:16Z
url: https://github.com/astral-sh/uv/pull/3318
synced_at: 2026-01-10T14:37:54Z
```

# Add `UV_NO_BUILD_ISOLATION` as environment variable

---

_Pull request opened by @flyaroundme on 2024-04-30 03:57_

## Summary
Hi! Added `UV_NO_BUILD_ISOLATION` as a boolean environment variable for the `--no-build-isolation` command-line option.

Closes https://github.com/astral-sh/uv/issues/3309

## Test Plan

Added new test `respect_no_build_isolation_env_var` to check that the behaviour is the same as if the ``--no-build-isolation`` command-line-option is set.


---

_@charliermarsh approved on 2024-04-30 04:01_

Thanks, this looks reasonable to me.

---

_@charliermarsh reviewed on 2024-04-30 04:01_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_install.rs`:1305 on 2024-04-30 04:01_

Why this change? I'll roll back.

---

_@flyaroundme reviewed on 2024-04-30 04:08_

---

_Review comment by @flyaroundme on `crates/uv/tests/pip_install.rs`:1305 on 2024-04-30 04:08_

Sorry, that was a typo

---

_Label `configuration` added by @charliermarsh on 2024-04-30 04:12_

---

_@flyaroundme reviewed on 2024-04-30 04:13_

---

_Review comment by @flyaroundme on `crates/uv/tests/pip_install.rs`:1305 on 2024-04-30 04:13_

Oh, it wasn't actually. It was a weird behaviour of typos pre-commit hook

---

_Merged by @charliermarsh on 2024-04-30 04:18_

---

_Closed by @charliermarsh on 2024-04-30 04:18_

---

_Comment by @zanieb on 2024-04-30 13:29_

Should usage here be `UV_BUILD_ISOLATION=false` instead?

---
