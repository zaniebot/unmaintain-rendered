```yaml
number: 16244
title: Add required environment marker example to hint
type: pull_request
state: merged
author: Prhmma
labels:
  - error messages
assignees: []
merged: true
base: main
head: fix-required-env-hint-example
created_at: 2025-10-10T23:14:24Z
updated_at: 2025-10-20T11:08:10Z
url: https://github.com/astral-sh/uv/pull/16244
synced_at: 2026-01-10T06:36:15Z
```

# Add required environment marker example to hint

---

_Pull request opened by @Prhmma on 2025-10-10 23:14_

## Summary
fixes issue #15938 
- show platform wheel hint with a concrete `tool.uv.required-environments` example so users know how to configure compatibility
- add `WheelTagHint::suggest_environment_marker` to pick a sensible environment marker based on the available wheel tags
- update the `sync_required_environment_hint` integration snapshot to expect the new multi-line hint

## Test Plan

cargo test --package uv --test it -- sync::sync_required_environment_hint


---

_@konstin reviewed on 2025-10-15 12:57_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/mod.rs`:5377 on 2025-10-15 12:57_

Why are we going through the tags here, instead of using the actual interpreter environment?

---

_@Prhmma reviewed on 2025-10-15 13:16_

---

_Review comment by @Prhmma on `crates/uv-resolver/src/lock/mod.rs`:5377 on 2025-10-15 13:16_

Thanks for the comment,
You are right, my initial understanding was the function is called when there are NO compatible wheels available for the current environment. In this case, we want to suggest what environment markers the user could configure to make the available wheels compatible.

but the function should use the actual interpreter environment (best) instead of the available wheel tags (tags). I will push the fix shortly

---

_@Prhmma reviewed on 2025-10-15 20:24_

---

_Review comment by @Prhmma on `crates/uv-resolver/src/lock/mod.rs`:5377 on 2025-10-15 20:24_

@konstin
 I made some changes and pushed a new commit, but it got more complicated than I expected, because I had to pipe the value across for the environment, which means more changes for a UX issue.
Please let me know your thoughts.

Thanks

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/mod.rs`:1455 on 2025-10-16 20:00_

nit: We generally do `use uv_pep508::MarkerEnvironment` and then use `&MarkerEnvironment`

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/mod.rs`:5397 on 2025-10-16 20:04_

When would this markers be empty?

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/mod.rs`:5587 on 2025-10-16 20:27_

Showing multiline code blocks in the terminal is hard, and we currently don't have a good general diagnostic system (like showing diffs for sections we want to change). For example, with the current layout, you'd get extra indentation when copying, while triple backticks look out-of-place in terminal output, etc. What about rolling this into a single line to avoid the formatting question?
> consider adding `"sys_platform == '[PLATFORM]' and platform_machine == '[MACHINE]'"` to `tool.uv.required-environments` to ensure uv resolves to a version with compatible wheels`

---

_@konstin reviewed on 2025-10-16 20:27_

---

_Label `error messages` added by @konstin on 2025-10-16 20:28_

---

_Renamed from "fix: add required environment marker example to hint" to "Add required environment marker example to hint" by @konstin on 2025-10-16 20:28_

---

_@Prhmma reviewed on 2025-10-17 10:57_

---

_Review comment by @Prhmma on `crates/uv-resolver/src/lock/mod.rs`:5397 on 2025-10-17 10:57_

This is Python output

```
>>> import platform
>>> help(platform.machine)
Help on function machine in module platform:

machine()
    Returns the machine type, e.g. 'i386'

    An empty string is returned if the value cannot be determined.
```


This is the defensive line, just to make sure we are safe.

---

_@Prhmma reviewed on 2025-10-17 11:07_

---

_Review comment by @Prhmma on `crates/uv-resolver/src/lock/mod.rs`:5587 on 2025-10-17 11:07_

pushed the changes, thanks


---

_Review comment by @konstin on `crates/uv-resolver/src/lock/mod.rs`:5397 on 2025-10-20 11:07_

Interesting, I don't think I've ever seen this value be empty, but it makes sense to keep it since it's documented.

---

_@konstin reviewed on 2025-10-20 11:07_

---

_@konstin approved on 2025-10-20 11:07_

Thank you!

---

_Merged by @konstin on 2025-10-20 11:08_

---

_Closed by @konstin on 2025-10-20 11:08_

---
