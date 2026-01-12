```yaml
number: 17996
title: "[ty] Add test coverage for `PythonEnvironment::System` variants"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
  - testing
  - ty
assignees: []
merged: true
base: main
head: zb/python-env-test
created_at: 2025-05-09T21:10:46Z
updated_at: 2025-05-10T20:28:34Z
url: https://github.com/astral-sh/ruff/pull/17996
synced_at: 2026-01-12T15:56:09Z
```

# [ty] Add test coverage for `PythonEnvironment::System` variants

---

_@zanieb_

Adds test coverage for https://github.com/astral-sh/ruff/pull/17991, which includes some minor refactoring of the virtual environment test infrastructure.

I tried to minimize stylistic changes, but there are still a few because I was a little confused by the setup. I could see this evolving more in the future, as I don't think the existing model can capture all the test coverage I'm looking for.

---

_Review requested from @carljm by @zanieb on 2025-05-09 21:10_

---

_Review requested from @AlexWaygood by @zanieb on 2025-05-09 21:10_

---

_Review requested from @sharkdp by @zanieb on 2025-05-09 21:10_

---

_Review requested from @dcreager by @zanieb on 2025-05-09 21:10_

---

_Comment by @github-actions[bot] on 2025-05-09 21:14_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Label `ty` added by @zanieb on 2025-05-09 22:01_

---

_Label `internal` added by @zanieb on 2025-05-09 22:01_

---

_Label `testing` added by @zanieb on 2025-05-09 22:01_

---

_Renamed from "Add test coverage for `PythonEnvironment::System` variants" to "[ty] Add test coverage for `PythonEnvironment::System` variants" by @zanieb on 2025-05-10 12:54_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/site_packages.rs`:854 on 2025-05-10 14:19_

this looks repeated from the virtual environment branch -- worth factoring it out into a helper function?

---

_@AlexWaygood approved on 2025-05-10 14:20_

---

_@zanieb reviewed on 2025-05-10 14:39_

---

_Review comment by @zanieb on `crates/ty_python_semantic/src/site_packages.rs`:854 on 2025-05-10 14:39_

It feels low priority to me in a test setup, but yeah we could.

---

_@AlexWaygood reviewed on 2025-05-10 17:13_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/site_packages.rs`:854 on 2025-05-10 17:13_

Yeah, I just don't like how these strings are pretty complicated and are duplicated in both branches -- it feels kinda fragile. But I don't feel strongly; feel free to disregard if you don't think it's worth spending time on!

---

_@zanieb reviewed on 2025-05-10 20:26_

---

_Review comment by @zanieb on `crates/ty_python_semantic/src/site_packages.rs`:854 on 2025-05-10 20:26_

The test setup feels fragile to me, I think that's why I wasn't motivated to do more consolidation. I can look at doing so in a follow-up though.

---

_@AlexWaygood reviewed on 2025-05-10 20:27_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/site_packages.rs`:854 on 2025-05-10 20:27_

it _is_ fragile, yeah, agreed. improvements very much welcome :-)

---

_Merged by @zanieb on 2025-05-10 20:28_

---

_Closed by @zanieb on 2025-05-10 20:28_

---

_Branch deleted on 2025-05-10 20:28_

---
