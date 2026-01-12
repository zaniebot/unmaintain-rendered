```yaml
number: 18003
title: "[ty] Do not allow invalid virtual environments from discovered `.venv` or `VIRTUAL_ENV`"
type: pull_request
state: merged
author: zanieb
labels:
  - ty
assignees: []
merged: true
base: main
head: zb/python-env-origin
created_at: 2025-05-10T12:52:09Z
updated_at: 2025-05-10T20:36:30Z
url: https://github.com/astral-sh/ruff/pull/18003
synced_at: 2026-01-12T15:56:10Z
```

# [ty] Do not allow invalid virtual environments from discovered `.venv` or `VIRTUAL_ENV`

---

_@zanieb_

Follow-up to https://github.com/astral-sh/ruff/pull/17991 ensuring we do not allow detection of system environments when the origin is `VIRTUAL_ENV` or a discovered `.venv` directory — i.e., those always require a `pyvenv.cfg` file.

---

_Review requested from @carljm by @zanieb on 2025-05-10 12:52_

---

_Review requested from @AlexWaygood by @zanieb on 2025-05-10 12:52_

---

_Review requested from @sharkdp by @zanieb on 2025-05-10 12:52_

---

_Review requested from @dcreager by @zanieb on 2025-05-10 12:52_

---

_@zanieb reviewed on 2025-05-10 12:53_

---

_Review comment by @zanieb on `crates/ty_python_semantic/src/site_packages.rs`:539 on 2025-05-10 12:53_

We could also frame this as `allows_system_environment` — no strong feelings here, easy to change as needed in the future.

---

_Label `ty` added by @zanieb on 2025-05-10 12:54_

---

_Comment by @github-actions[bot] on 2025-05-10 13:00_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/site_packages.rs`:728 on 2025-05-10 14:25_

```suggestion
        #[track_caller]
        fn err(self) -> SitePackagesDiscoveryError {
            PythonEnvironment::new(self.build(), self.origin, &self.system)
                .expect_err("Expected site-packages discovery to fail")
        }
```

(`expect_err()` prints the contents of the `Ok()` variant in the panic message, as well as the string you pass into the function)

---

_@AlexWaygood approved on 2025-05-10 14:27_

---

_@zanieb reviewed on 2025-05-10 14:40_

---

_Review comment by @zanieb on `crates/ty_python_semantic/src/site_packages.rs`:728 on 2025-05-10 14:40_

👍 

---

_@zanieb reviewed on 2025-05-10 14:40_

---

_Review comment by @zanieb on `crates/ty_python_semantic/src/site_packages.rs`:728 on 2025-05-10 14:40_

Why `#[track_caller]`?

---

_@AlexWaygood reviewed on 2025-05-10 17:19_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/site_packages.rs`:728 on 2025-05-10 17:19_

> Why `#[track_caller]`?

if the `expect_err()` call fails, the panic message will give the line number that triggered the panic as being some line inside the `PythonEnvironmentTestCase::err()` function, which isn't very helpful if you're trying to figure out which test failed. You probably actually want the panic message to tell you the line number of the `#[test]`-decorated function that called `PythonEnvironmentTestCase::run()`. Then the panic message gives you a line number that allows you to jump straight to the failing test.

To do that, we'd also need to add `#[track_caller]` to `PythonEnvironmentTestCase::run()` as well as `PythonEnvironmentTestCase::err()`, though.

See also:
- https://doc.rust-lang.org/reference/attributes/codegen.html#the-track_caller-attribute
- @sharkdp's excellent examples and explanation of what this attribute is good for in https://github.com/astral-sh/ruff/pull/13884#issue-2607736129

---

_@zanieb reviewed on 2025-05-10 20:32_

---

_Review comment by @zanieb on `crates/ty_python_semantic/src/site_packages.rs`:728 on 2025-05-10 20:32_

That's nice!

---

_Merged by @zanieb on 2025-05-10 20:36_

---

_Closed by @zanieb on 2025-05-10 20:36_

---

_Branch deleted on 2025-05-10 20:36_

---
