```yaml
number: 1040
title: Prefer target Python version over current version for builds
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/target-build-version
created_at: 2024-01-22T18:14:53Z
updated_at: 2024-01-24T17:35:35Z
url: https://github.com/astral-sh/uv/pull/1040
synced_at: 2026-01-12T16:04:22Z
```

# Prefer target Python version over current version for builds

---

_@zanieb_

Extends #1029 
Closes https://github.com/astral-sh/puffin/issues/1038

Instead of always using the current Python version for builds when a target version is provided, we will do our best to use a compatible Python version for builds.

Removes behavior where Python versions without patch versions were always assumed to be the latest known patch version (previously discussed in https://github.com/astral-sh/puffin/pull/534). While this was convenient for resolutions which include packages which require minimum patch versions e.g. `requires-python=">=3.7.4"`, it conflicts with the idea that the target Python version you provide is the _minimum_ compatible version. Additionally, it complicates interpreter lookup as we cannot tell if the user has asked for that specific patch version or not.



---

_@zanieb reviewed on 2024-01-22 19:30_

---

_Review comment by @zanieb on `crates/puffin-interpreter/src/interpreter.rs`:117 on 2024-01-22 19:30_

We could consider changing this method to return `None` here and let the caller choose this behavior explicitly?

---

_Comment by @zanieb on 2024-01-23 22:00_

This introduces some unfortunately complexity to our Python version tests in `pip compile` since we now need to isolate scenarios to a certain subset of available Python versions. I think that should be tackled separately from this work.

---

_Marked ready for review by @zanieb on 2024-01-23 22:00_

---

_@zanieb reviewed on 2024-01-23 22:02_

---

_Review comment by @zanieb on `crates/puffin/tests/pip_compile_scenarios.rs`:319 on 2024-01-23 22:02_

While this test is no longer working as intended, it's great to see it passing! Now that we build with a matching version if it's available, the requirements are satisfiable in CI. We'll need to introduce some more complexity to test error messages in this case.

---

_Review requested from @charliermarsh by @zanieb on 2024-01-23 22:47_

---

_@charliermarsh reviewed on 2024-01-24 00:46_

---

_Review comment by @charliermarsh on `crates/puffin-interpreter/src/interpreter.rs`:117 on 2024-01-24 00:46_

I think I would prefer something like that. It was a little confusing to me that the logic below in this method still relies on `python_version`, since shouldn't that be covered by `find_strict`? I'm talking about, e.g., `if let Some(python_version) = python_version {` below.

In other words, wondering if we have one method that takes `python_version: &PythonVersion` and returns `Result<Option<Self>>`, and then another that doesn't take `python_version` and returns `Result<Self>`. Then we can do some `map` or `and_then` or whatever at the call site.

---

_Review comment by @konstin on `crates/puffin-interpreter/src/interpreter.rs`:153 on 2024-01-24 11:00_

Can we move this onto `PythonVersion` as something like `PythonVersion::is_compatible`? 

---

_Review comment by @konstin on `crates/puffin-interpreter/src/interpreter.rs`:165 on 2024-01-24 11:04_

I'm currently going for `if cfg!(...)` with a final `unimplemented!(...)` branch in the windows compat changes; The idea is that you get coverage for all platforms with a local `cargo check` and it avoids sprinkling `allow(unused)` over platform dependent code. 

---

_@konstin approved on 2024-01-24 11:07_

I think we should unconditionally print a line about the used installed and target python interpreter when using `--python-version`, this will avoid confusion about source distribution building.

---

_@zanieb reviewed on 2024-01-24 15:08_

---

_Review comment by @zanieb on `crates/puffin-interpreter/src/interpreter.rs`:153 on 2024-01-24 15:08_

Done!

---

_@konstin reviewed on 2024-01-24 15:12_

---

_Review comment by @konstin on `crates/puffin-interpreter/src/interpreter.rs`:165 on 2024-01-24 15:12_

See #940

---

_@zanieb reviewed on 2024-01-24 15:15_

---

_Review comment by @zanieb on `crates/puffin-interpreter/src/interpreter.rs`:165 on 2024-01-24 15:15_

Makes sense, didn't mean to change that.

---

_@zanieb reviewed on 2024-01-24 15:17_

---

_Review comment by @zanieb on `crates/puffin-interpreter/src/interpreter.rs`:117 on 2024-01-24 15:17_

It's a bit different than what's presented here, but I think what I have now is much better.

---

_@charliermarsh reviewed on 2024-01-24 15:19_

---

_Review comment by @charliermarsh on `crates/puffin-interpreter/src/interpreter.rs`:101 on 2024-01-24 15:19_

I still find the structure here a bit confusing, because `find_version` takes an Option.

I think I would've expected something like `find_version` taking a non-Option, and then here...

```
if let Some(python_version) = python_version {
    if let Some(interpreter) = Self::find_version(...) {
        return Ok(interpreter);
    }
    
    // If that fails, try with a different patch
    ...
} else {
   Self::find_generic(platform, cache)
}
```



---

_@zanieb reviewed on 2024-01-24 15:21_

---

_Review comment by @zanieb on `crates/puffin-interpreter/src/interpreter.rs`:101 on 2024-01-24 15:21_

Then we need to duplicate all of the logic for looking up Python interpreters at given locations, which seems more brittle.

---

_Merged by @zanieb on 2024-01-24 17:12_

---

_Closed by @zanieb on 2024-01-24 17:12_

---

_Branch deleted on 2024-01-24 17:12_

---

_Review comment by @konstin on `crates/puffin-interpreter/src/interpreter.rs`:108 on 2024-01-24 17:13_

Does this mean we ignore an explicit patch version request if it can't be fulfilled, even it means picking an incompatible patch version?

---

_@konstin reviewed on 2024-01-24 17:13_

---

_Review comment by @charliermarsh on `crates/puffin-interpreter/src/interpreter.rs`:101 on 2024-01-24 17:31_

This is what I was thinking, but I defer to you: https://github.com/astral-sh/puffin/pull/1078/files

---

_@charliermarsh reviewed on 2024-01-24 17:31_

---

_@charliermarsh reviewed on 2024-01-24 17:31_

---

_Review comment by @charliermarsh on `crates/puffin-interpreter/src/interpreter.rs`:95 on 2024-01-24 17:31_

Nit: `find_strict` no longer exists.

---

_Review comment by @charliermarsh on `crates/puffin-interpreter/src/interpreter.rs`:137 on 2024-01-24 17:31_

Nit: this can be private.

---

_@charliermarsh reviewed on 2024-01-24 17:31_

---

_Review comment by @zanieb on `crates/puffin-interpreter/src/interpreter.rs`:108 on 2024-01-24 17:33_

Unless you call `find_version` directly, yeah. #1072 will warn the user in this case.

We will still use the patch version requested for resolution, we just won't build with a matching version.

---

_@zanieb reviewed on 2024-01-24 17:33_

---

_@zanieb reviewed on 2024-01-24 17:34_

---

_Review comment by @zanieb on `crates/puffin-interpreter/src/interpreter.rs`:95 on 2024-01-24 17:34_

https://github.com/astral-sh/puffin/pull/1079

---

_@zanieb reviewed on 2024-01-24 17:35_

---

_Review comment by @zanieb on `crates/puffin-interpreter/src/interpreter.rs`:137 on 2024-01-24 17:35_

This was intentional — I think this makes sense as part of the public interface if you actually need a specific version.

---
