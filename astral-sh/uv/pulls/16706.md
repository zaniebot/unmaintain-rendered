```yaml
number: 16706
title: "Fix handling of `python install --default` for pre-release Python versions"
type: pull_request
state: merged
author: nooscraft
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: fix-default-prerelease
created_at: 2025-11-12T14:56:44Z
updated_at: 2025-11-12T18:34:08Z
url: https://github.com/astral-sh/uv/pull/16706
synced_at: 2026-01-10T05:58:11Z
```

# Fix handling of `python install --default` for pre-release Python versions

---

_Pull request opened by @nooscraft on 2025-11-12 14:56_

## Summary

Fixes `--default` not creating default executable links for pre-release Python versions.

When using `--default` with a pre-release version like `3.15.0a1`, the code was checking `matches_installation()` against the download request instead of the original user request. This caused the check to fail since the download request doesn't match pre-release versions the same way.

Changed it to use `installation.satisfies(&first_request.request)` when `--default` is used, which checks against the original user request.

Fixes #16696

## Test Plan

Added `python_install_default_prerelease` test that installs Python 3.15 with `--default` and verifies all three executable links (`python3.15`, `python3`, `python`) are created. The test skips gracefully if 3.15 isn't available.

All existing tests pass.


---

_@zanieb reviewed on 2025-11-12 15:40_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:789 on 2025-11-12 15:40_

When `--default` is used, we'll only allow one request anyway, right? so we could skip the check entirely?

---

_Review comment by @nooscraft on `crates/uv/src/commands/python/install.rs`:789 on 2025-11-12 16:00_

@zanieb you're right. Since `--default` only allows one request, all installations in the loop already match that request, so the check is redundant. I've simplified it to skip the check when default is true. All tests still pass.

---

_@nooscraft reviewed on 2025-11-12 16:00_

---

_Comment by @zanieb on 2025-11-12 17:54_

Sorry another question here... isn't the entire `first_request` matching meaningless then? Can we just remove that.. ?

---

_Comment by @nooscraft on 2025-11-12 18:07_

> Sorry another question here... isn't the entire `first_request` matching meaningless then? Can we just remove that.. ?

Yes, When is_default_install is true, there's only one request and all installations in the loop already match it, so the check was redundant.
I've removed the `first_request.matches_installation(installation)` check and the unused `first_request` variable. The code now just checks `default || (is_default_install && preview.is_enabled(...))`.

---

_@zanieb reviewed on 2025-11-12 18:11_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_install.rs`:2280 on 2025-11-12 18:11_

We should `.assert().success()` here instead. 3.15 _is_ available, we shouldn't let the test silently fail.

---

_@zanieb reviewed on 2025-11-12 18:11_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_install.rs`:2305 on 2025-11-12 18:11_

We don't need this, the tests run in isolated contexts

---

_@zanieb reviewed on 2025-11-12 18:12_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_install.rs`:2262 on 2025-11-12 18:12_

This belongs in a doc comment `/// ` above the function

---

_@nooscraft reviewed on 2025-11-12 18:19_

---

_Review comment by @nooscraft on `crates/uv/tests/it/python_install.rs`:2280 on 2025-11-12 18:19_

Yup, fixed this!

---

_@nooscraft reviewed on 2025-11-12 18:20_

---

_Review comment by @nooscraft on `crates/uv/tests/it/python_install.rs`:2305 on 2025-11-12 18:20_

Done!

---

_@nooscraft reviewed on 2025-11-12 18:20_

---

_Review comment by @nooscraft on `crates/uv/tests/it/python_install.rs`:2262 on 2025-11-12 18:20_

Moved to doc comment

---

_Renamed from "Fix --default flag for pre-release Python versions" to "Fix handling of `python install --default` for pre-release Python versions" by @zanieb on 2025-11-12 18:33_

---

_@zanieb approved on 2025-11-12 18:33_

---

_Comment by @zanieb on 2025-11-12 18:33_

Thanks!

---

_Merged by @zanieb on 2025-11-12 18:33_

---

_Closed by @zanieb on 2025-11-12 18:33_

---

_Label `bug` added by @zanieb on 2025-11-12 18:34_

---

_Label `preview` added by @zanieb on 2025-11-12 18:34_

---
