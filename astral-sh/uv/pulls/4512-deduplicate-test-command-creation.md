```yaml
number: 4512
title: Deduplicate test command creation
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/deduplicate-test-command-creation
created_at: 2024-06-25T12:56:13Z
updated_at: 2024-06-25T22:06:55Z
url: https://github.com/astral-sh/uv/pull/4512
synced_at: 2026-01-10T13:48:28Z
```

# Deduplicate test command creation

---

_Pull request opened by @konstin on 2024-06-25 12:56_

This PR refactors the command creation in the test suite to remove the duplication.

**1)** We add the same set of test stubbing args to almost any uv invocation in the tests:

```rust
command
    .arg("--cache-dir")
    .arg(self.cache_dir.path())
    .env("VIRTUAL_ENV", self.venv.as_os_str())
    .env("UV_NO_WRAP", "1")
    .env("HOME", self.home_dir.as_os_str())
    .env("UV_TOOLCHAIN_DIR", "")
    .env("UV_TEST_PYTHON_PATH", &self.python_path())
    .current_dir(self.temp_dir.path());

if cfg!(all(windows, debug_assertions)) {
    // TODO(konstin): Reduce stack usage in debug mode enough that the tests pass with the
    // default windows stack of 1MB
    command.env("UV_STACK_SIZE", (8 * 1024 * 1024).to_string());
}
```

Centralizing these into a `TestContext::add_shared_args` method removes them from everywhere.

**2)** Prefix all `TextContext` methods of the pip interface with `pip_`. This is now necessary due to `uv sync` vs. `uv pip sync`.

**3)** Move command creation in the various test files into dedicated functions or methods to avoid repeating the arguments. Except for error message tests, there should be at most one `Command::new(get_bin())` call per test file. `EXCLUDE_NEWER` is exclusively used in `TestContext`.

---

I'm considering adding a `TestCommand` on top of these changes (in another PR) that holds a reference to the `TextContext`, has `add_shared_args` as a method and uses `Fn(Self) -> Self` instead of `Fn(&mut Self) -> Self` for methods to improved chaining.


---

_Label `internal` added by @konstin on 2024-06-25 12:56_

---

_Review requested from @zanieb by @konstin on 2024-06-25 12:56_

---

_@zanieb reviewed on 2024-06-25 13:11_

---

_Review comment by @zanieb on `crates/uv/tests/pip_check.rs`:14 on 2024-06-25 13:11_

Shouldn't we just have these in the test context itself?

---

_@zanieb reviewed on 2024-06-25 13:13_

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:343 on 2024-06-25 13:13_

Honestly I don't think we need a dedicated method for this. People should just `unset_env("EXCLUDE_NEWER")` in the relevant tests?

---

_@zanieb approved on 2024-06-25 13:13_

Thank you! This was on my backlog :)

---

_@konstin reviewed on 2024-06-25 21:08_

---

_Review comment by @konstin on `crates/uv/tests/common/mod.rs`:343 on 2024-06-25 21:08_

https://github.com/astral-sh/uv/issues/4531

---

_@konstin reviewed on 2024-06-25 21:10_

---

_Review comment by @konstin on `crates/uv/tests/pip_check.rs`:14 on 2024-06-25 21:10_

I feel they are fine, feel free to move them though

---

_Merged by @konstin on 2024-06-25 22:06_

---

_Closed by @konstin on 2024-06-25 22:06_

---

_Branch deleted on 2024-06-25 22:06_

---
