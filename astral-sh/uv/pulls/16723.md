```yaml
number: 16723
title: "tests: cache managed Python installs"
type: pull_request
state: open
author: nooscraft
labels: []
assignees: []
base: main
head: fix-managed-python-cache-tests
created_at: 2025-11-13T15:06:06Z
updated_at: 2025-11-14T02:55:37Z
url: https://github.com/astral-sh/uv/pull/16723
synced_at: 2026-01-10T05:58:11Z
```

# tests: cache managed Python installs

---

_Pull request opened by @nooscraft on 2025-11-13 15:06_

Cache managed Python downloads (#16721)

---

_Review comment by @konstin on `crates/uv/tests/it/python_install.rs`:2687 on 2025-11-13 15:34_

Why are we using the cache dir in the offline test?

---

_Review comment by @konstin on `crates/uv/tests/it/python_install.rs`:2713 on 2025-11-13 15:34_

Why this change?

---

_@konstin reviewed on 2025-11-13 15:34_

---

_Comment by @konstin on 2025-11-13 15:34_

How fast are the tests before and after, does the cache work?

---

_@nooscraft reviewed on 2025-11-13 15:44_

---

_Review comment by @nooscraft on `crates/uv/tests/it/python_install.rs`:2687 on 2025-11-13 15:44_

`with_managed_python_dirs()` now calls `with_python_download_cache()`, so without an override the offline test inherits the warm cache and starts passing when earlier tests download that version. Pointing it at a dedicated temp cache keeps the test deterministic, the offline install still fails because the cache is empty, which is what the test asserts.


---

_@nooscraft reviewed on 2025-11-13 15:44_

---

_Review comment by @nooscraft on `crates/uv/tests/it/python_install.rs`:2713 on 2025-11-13 15:44_

Same reason as above,once the helper enables the shared cache by default we need an explicit optout. The new `without_python_download_cache()` clears the shared value and replaces it with an empty per test cache so no cache scenaris keep utilizing the intended code path.

---

_Comment by @nooscraft on 2025-11-13 15:47_

> How fast are the tests before and after, does the cache work?

I ran `cargo test --package uv --test it python_install -- --nocapture` before and after. The very first one still takes about 42 s because we start with an empty cache, but every run after that drops into the 38–39 s range, managed downloads get reused and we stop pulling over the network each time.

---

_Review comment by @zanieb on `crates/uv/tests/it/common/mod.rs`:489 on 2025-11-13 15:58_

I think I'd expect this to no longer be `pub` and all the redundant calls to be dropped

---

_@zanieb reviewed on 2025-11-13 15:58_

---

_@zanieb reviewed on 2025-11-13 15:58_

---

_Review comment by @zanieb on `crates/uv/tests/it/common/mod.rs`:509 on 2025-11-13 15:58_

I don't think this can be an empty directory, we need to exercise the code path where the variable is unset

---

_Comment by @nooscraft on 2025-11-13 16:56_

Thanks for the feedback, @zanieb! I want to make sure I understand correctly before pushing the changes

I've made with_python_download_cache() non-public and removed all explicit calls from test files (python_install.rs, python_find.rs, python_upgrade.rs, and pip_install.rs).

Since with_managed_python_dirs() now calls it internally, tests using .with_managed_python_dirs() will automatically get the cache enabled. Is this the intended behavior you were looking for?

You mentioned we need to exercise the code path where the variable is unset. I've changed it to:
```
pub fn without_python_download_cache(mut self) -> Self {
    let key: OsString = EnvVars::UV_PYTHON_CACHE_DIR.into();
    self.extra_env.retain(|(existing, _)| existing != &key);
    self
}
```

This removes the variable from extra_env entirely, rather than setting it to an empty directory. Is this what you meant by unsetting the variable, or did you mean something else?

(I want to confirm this correctly tests the "cache disabled" scenario vs just testing "empty cache directory".)

---

_Comment by @nooscraft on 2025-11-14 02:55_

@konstin @zanieb I’ve pushed the changes so it’s easier for you to review what has actually changed, instead of going in circles. If this isn’t the expected outcome, it’s possible I misunderstood the intent behind the original change request. If that’s the case, I’m happy to hand this over to someone with a better understanding of the test and cache logic.

---
