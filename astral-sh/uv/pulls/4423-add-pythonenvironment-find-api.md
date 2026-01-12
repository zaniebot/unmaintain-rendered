```yaml
number: 4423
title: "Add `PythonEnvironment::find` API"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/toolchain-environment
created_at: 2024-06-20T13:57:30Z
updated_at: 2024-06-20T17:54:18Z
url: https://github.com/astral-sh/uv/pull/4423
synced_at: 2026-01-12T16:06:13Z
```

# Add `PythonEnvironment::find` API

---

_@zanieb_

Restores the `PythonEnvironment::find` API which was removed a while back in favor of `Toolchain::find`. As mentioned in #4416, I'm attempting to separate the case where you want an active environment from the case where you want an installed toolchain in order to create environments.

I wanted to drop `EnvironmentPreference` from `Toolchain::find` and just have us consistently consider (or not consider) virtual environments when discovering toolchains for creating environments. Unfortunately this caused a few things to break so I reverted that change and will explore it separately. Because I was exploring that change, there are some minor changes to the `Toolchain` API here.

---

_Label `internal` added by @zanieb on 2024-06-20 13:57_

---

_@zanieb reviewed on 2024-06-20 14:02_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/mod.rs`:186 on 2024-06-20 14:02_

Need to consider if this change is correct

---

_@zanieb reviewed on 2024-06-20 14:03_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip/uninstall.rs`:57 on 2024-06-20 14:03_

This is the gist of the change. We want an environment, not an installed toolchain. We don't need to specify toolchain preferences.

---

_@zanieb reviewed on 2024-06-20 14:12_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/lib.rs`:844 on 2024-06-20 14:12_

I dropped down to the lower level APIs in all of these tests for now since I'm futzing with the `Toolchain` API

---

_@zanieb reviewed on 2024-06-20 14:12_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/lib.rs`:844 on 2024-06-20 14:12_

These tests are going to move to the `uv toolchain find` API eventually

---

_Marked ready for review by @zanieb on 2024-06-20 15:48_

---

_Review requested from @BurntSushi by @zanieb on 2024-06-20 16:26_

---

_Review requested from @konstin by @zanieb on 2024-06-20 16:27_

---

_Review comment by @BurntSushi on `crates/uv-toolchain/src/environment.rs`:32 on 2024-06-20 16:43_

Maybe something to add to the docs here is what it means to call `find` here without a toolchain request. What is its behavior?

It looks like it does `unwrap_or_default()`, but I think I'd wonder whether it would be clearer to just ask for a `&ToolchainRequest` directly here and let callers explicitly provide the default if that's what they want? Not totally sure about this...

---

_@BurntSushi approved on 2024-06-20 16:44_

---

_@zanieb reviewed on 2024-06-20 17:09_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/environment.rs`:32 on 2024-06-20 17:09_

Good point. I'll ask for the toolchain request directly, I just tweaked the `Toolchain::find` API to do that.

---

_Comment by @codspeed-hq[bot] on 2024-06-20 17:27_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/zb/toolchain-environment)

### Merging #4423 will **degrade performances by 12.11%**

<sub>Comparing <code>zb/toolchain-environment</code> (caf31b0) with <code>main</code> (13e532c)</sub>



### Summary

`❌ 1` regressions
`✅ 12` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/zb/toolchain-environment)._

### Benchmarks breakdown

|     | Benchmark | `main` | `zb/toolchain-environment` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `resolve_warm_airflow` | 1.3 s | 1.5 s | -12.11% |


---

_Merged by @zanieb on 2024-06-20 17:54_

---

_Closed by @zanieb on 2024-06-20 17:54_

---

_Branch deleted on 2024-06-20 17:54_

---
