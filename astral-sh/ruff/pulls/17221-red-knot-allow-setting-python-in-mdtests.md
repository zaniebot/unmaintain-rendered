```yaml
number: 17221
title: "[red-knot] Allow setting `python` in mdtests"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/mdtest-python
created_at: 2025-04-05T14:32:06Z
updated_at: 2025-04-05T15:36:15Z
url: https://github.com/astral-sh/ruff/pull/17221
synced_at: 2026-01-10T19:40:37Z
```

# [red-knot] Allow setting `python` in mdtests

---

_Pull request opened by @MichaReiser on 2025-04-05 14:32_

## Summary

This PR extends the mdtest options to allow setting the `environment.python` option.

## Test Plan

I let @AlexWaygood write a test and he'll tell me if it works 😆 


---

_Review requested from @carljm by @MichaReiser on 2025-04-05 14:32_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-04-05 14:32_

---

_Review requested from @sharkdp by @MichaReiser on 2025-04-05 14:32_

---

_Review requested from @dcreager by @MichaReiser on 2025-04-05 14:32_

---

_Label `red-knot` added by @MichaReiser on 2025-04-05 14:32_

---

_Comment by @github-actions[bot] on 2025-04-05 14:34_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @AlexWaygood on 2025-04-05 14:35_

👍 the test I have locally passes when it's rebased on this branch

---

_@AlexWaygood approved on 2025-04-05 14:35_

---

_Comment by @AlexWaygood on 2025-04-05 14:36_

thanks!

---

_Comment by @AlexWaygood on 2025-04-05 14:37_

Oh no shoot, I messed it up locally. Actually it now fails with this when my PR branch is rebased onto this branch:

```
failures:

---- mdtest__import_relative stdout ----

thread 'mdtest__import_relative' panicked at crates/red_knot_test/src/lib.rs:247:6:
Failed to update Program settings in TestDb: .venv does not point to a directory
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

---

_Merged by @MichaReiser on 2025-04-05 14:43_

---

_Closed by @MichaReiser on 2025-04-05 14:43_

---

_Branch deleted on 2025-04-05 14:43_

---

_@AlexWaygood reviewed on 2025-04-05 15:03_

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/lib.rs`:239 on 2025-04-05 15:03_

Hmm, I'm still struggling to get my test to pass locally. I think this is incorrect because the `python` configuration setting is meant to point to the installation's `sys.prefix` value (e.g. `.venv`) but the `KnownSitePackages` variant of `PythonPath` is meant to hold the resolved `site-packages` subdirectory of `sys.prefix` (e.g. `.venv/lib/python3.13/site-packages` if it's a Python 3.13 virtual environment)

---

_@AlexWaygood reviewed on 2025-04-05 15:07_

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/lib.rs`:239 on 2025-04-05 15:07_

I think this should be using `PythonPath::SysPrefix` instead

---

_@AlexWaygood reviewed on 2025-04-05 15:22_

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/lib.rs`:239 on 2025-04-05 15:22_

I'll fix this as part of my PR

---

_@MichaReiser reviewed on 2025-04-05 15:32_

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/lib.rs`:239 on 2025-04-05 15:32_

🤦 yes, this should be `SysPrefix`, at least if it should mirror the CLI

---

_@AlexWaygood reviewed on 2025-04-05 15:36_

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/lib.rs`:239 on 2025-04-05 15:36_

Yeah, I'm writing a fix!

---
