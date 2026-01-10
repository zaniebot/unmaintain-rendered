```yaml
number: 4662
title: Set fork solution as preference when resolving
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
  - preview
assignees: []
merged: true
base: main
head: charlie/pref
created_at: 2024-06-30T19:20:26Z
updated_at: 2024-07-09T12:58:36Z
url: https://github.com/astral-sh/uv/pull/4662
synced_at: 2026-01-10T13:42:52Z
```

# Set fork solution as preference when resolving

---

_Pull request opened by @charliermarsh on 2024-06-30 19:20_

## Summary

This should both make it faster to solve forks (since we have a guess for a valid resolution, and will bias towards packages we've already fetched) and improve consistency between forks.

Closes https://github.com/astral-sh/uv/issues/4617.


---

_Label `performance` added by @charliermarsh on 2024-06-30 19:20_

---

_Label `preview` added by @charliermarsh on 2024-06-30 19:20_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-06-30 19:20_

---

_Review requested from @konstin by @charliermarsh on 2024-06-30 19:20_

---

_@charliermarsh reviewed on 2024-06-30 19:31_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/preferences.rs`:102 on 2024-06-30 19:31_

Does anyone know why this is in an `Arc`?

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:380 on 2024-07-01 01:43_

This is quadratic but presumedly there are a small number of forks...?

---

_@charliermarsh reviewed on 2024-07-01 01:43_

---

_@charliermarsh reviewed on 2024-07-01 01:44_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock_scenarios.rs`:1710 on 2024-07-01 01:44_

The comment on this test says:

```rust
/// So this acts as a regression test to ensure that only one version of `a` is selected.
```

But I guess it wasn't actually doing that!

---

_Review comment by @konstin on `crates/uv-resolver/src/preferences.rs`:102 on 2024-07-01 08:18_

This was added when moving the solver into its own thread @ibraheemdev 

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:380 on 2024-07-01 08:28_

What about updating the `ResolverState` preferences instead? This should update all forks implicitly.

---

_@konstin approved on 2024-07-01 12:21_

---

_Comment by @charliermarsh on 2024-07-01 12:22_

> What about updating the ResolverState preferences instead? This should update all forks implicitly.

That would require that the resolver state is taken mutably, but it's not here.

---

_Comment by @charliermarsh on 2024-07-01 12:23_

I could do it by wrapping `Preferences` in a Mutex or RwLock but that may have bigger perf implications - wdyt?

---

_Merged by @charliermarsh on 2024-07-01 12:25_

---

_Closed by @charliermarsh on 2024-07-01 12:25_

---

_Branch deleted on 2024-07-01 12:25_

---

_Comment by @konstin on 2024-07-01 12:34_

We could try a mutable `Preferences` outside the `'FORK` scope that we pass around, this should get us the same effect without any performance implications.

---

_Comment by @charliermarsh on 2024-07-01 12:35_

That's true, I can try that.

---

_@BurntSushi reviewed on 2024-07-09 12:58_

---

_Review comment by @BurntSushi on `crates/uv/tests/lock_scenarios.rs`:1710 on 2024-07-09 12:58_

Yeah I think we need a way to assert the result in the packse scenario itself. Otherwise it's pretty easy to miss if the result of the test isn't what you expected it to be.

---
