```yaml
number: 16928
title: "Noisily allow redundant entries in `tool.uv.build-backend.module-name`"
type: pull_request
state: merged
author: EliteTK
labels:
  - bug
assignees: []
merged: true
base: main
head: dedupe-mod-names
created_at: 2025-12-02T12:08:50Z
updated_at: 2025-12-03T11:10:44Z
url: https://github.com/astral-sh/uv/pull/16928
synced_at: 2026-01-12T16:12:31Z
```

# Noisily allow redundant entries in `tool.uv.build-backend.module-name`

---

_@EliteTK_

## Summary

Fix #16906 by pruning modules or submodules which are already included (either directly, or through a parent).

Generates warnings when this happens.

Example:

```bash session
$ uv build
Building source distribution (uv build backend)...
warning: Ignoring redundant module name(s): test_lib.bar test_lib test_lib.bar.baz test_lib.baz
Building wheel from source distribution (uv build backend)...
Successfully built dist/test-0.1.0.tar.gz
Successfully built dist/test-0.1.0-py3-none-any.whl
```

## Test Plan

Added some unit tests for the pruning function and one for the whole build backend. Added an integration test for the warnings. Ran the full test suite. Manually tested.

The unit test for the function doesn't cater for the fact that it doesn't guarantee an order at the moment. I think this is fine.

## Outstanding/Questions

* Figure out how to only present warnings when appropriate (when building something the user controls).

---

_Review requested from @konstin by @EliteTK on 2025-12-02 12:08_

---

_Label `bug` added by @EliteTK on 2025-12-02 12:08_

---

_Review comment by @konstin on `crates/uv-build-backend/src/lib.rs`:229 on 2025-12-02 12:22_

We only show this warning if it's a source distribution the user controls, not for e.g. arbitrary packages from PyPI. The best proxy to use for this is `SourceStrategy`, if it's enabled we consider it something to warn about.

---

_Review comment by @konstin on `crates/uv-build-backend/src/lib.rs`:1466 on 2025-12-02 12:25_

We don't usually use single character names for functions. An easier approach here would be a function with two arguments that does the assert inside it.

---

_@konstin reviewed on 2025-12-02 12:25_

---

_@EliteTK reviewed on 2025-12-02 13:30_

---

_Review comment by @EliteTK on `crates/uv-build-backend/src/lib.rs`:1466 on 2025-12-02 13:30_

Thanks for the suggestion. I've changed this.

---

_@EliteTK reviewed on 2025-12-02 14:12_

---

_Review comment by @EliteTK on `crates/uv-build-backend/src/lib.rs`:229 on 2025-12-02 14:12_

Does `uv-build` have enough information to determine the `SourceStrategy`?

---

_Review requested from @konstin by @EliteTK on 2025-12-02 14:25_

---

_Review comment by @konstin on `crates/uv-build-backend/src/lib.rs`:229 on 2025-12-02 14:27_

Every time we call `uv-build`, the caller knows the sources strategy, which we can use, see also https://github.com/astral-sh/uv/pull/15750 for slightly different case that also checks the source strategy.

---

_@konstin reviewed on 2025-12-02 14:27_

---

_@zanieb reviewed on 2025-12-02 14:47_

---

_Review comment by @zanieb on `crates/uv-build-backend/src/lib.rs`:242 on 2025-12-02 14:47_

This is a nit, but we like to get the detail right instead of using "(s)"; so split the message into plural and singular forms based on the number of items. e.g., https://github.com/astral-sh/uv/blob/fd7e6d0a057f7db56b124565c92d847d411fd39e/crates/uv/src/commands/mod.rs#L197

---

_Review comment by @zanieb on `crates/uv-build-backend/src/lib.rs`:236 on 2025-12-02 14:47_

```suggestion
/// Wraps [`prune_redundant_modules`] with a warning when modules are ignored
```

---

_@zanieb reviewed on 2025-12-02 14:47_

---

_Marked ready for review by @EliteTK on 2025-12-02 17:17_

---

_@EliteTK reviewed on 2025-12-02 17:20_

---

_Review comment by @EliteTK on `crates/uv-build-backend/src/lib.rs`:229 on 2025-12-02 17:20_

Alright, I've implemented this in [Make warnings conditional](https://github.com/astral-sh/uv/pull/16928/commits/9e68685882e2c579c356efa4067ee3e79ec10be2).

There are two cases where there isn't enough information for this: the hidden build-backend subcommand and the uv-build binary. So I just pass false there (and in a bunch of tests).

---

_Review comment by @konstin on `crates/uv-build-backend/src/lib.rs`:245 on 2025-12-03 09:22_

```suggestion
            "Ignoring redundant module name{s} in `tool.uv.build-backend.module-name`: `{}`",
            ignored.into_iter().join("`, `")
```

---

_@konstin reviewed on 2025-12-03 09:23_

---

_@konstin approved on 2025-12-03 09:27_

---

_Merged by @EliteTK on 2025-12-03 10:05_

---

_Closed by @EliteTK on 2025-12-03 10:05_

---

_Branch deleted on 2025-12-03 11:10_

---
