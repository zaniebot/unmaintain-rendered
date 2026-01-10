```yaml
number: 5014
title: "`uv tool run` error messages references `uvx` when appropriate"
type: pull_request
state: merged
author: blueraft
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: reference-uvx
created_at: 2024-07-12T15:40:47Z
updated_at: 2024-07-12T17:17:27Z
url: https://github.com/astral-sh/uv/pull/5014
synced_at: 2026-01-10T13:42:52Z
```

# `uv tool run` error messages references `uvx` when appropriate

---

_Pull request opened by @blueraft on 2024-07-12 15:40_

## Summary

Resolves #5013. 

## Test Plan

```console
❯ ./target/debug/uv tool run fastapi-cli
warning: `uv tool run` is experimental and may change without warning.
Resolved 9 packages in 28ms
The executable fastapi-cli was not found.
However, the following executables are available via uv tool run --from fastapi-cli <EXECUTABLE>:
- fastapi
```

```console
❯ ./target/debug/uvx fastapi-cli
warning: `uvx` is experimental and may change without warning.
Resolved 9 packages in 23ms
The executable fastapi-cli was not found.
However, the following executables are available via uvx --from fastapi-cli <EXECUTABLE>:
- fastapi
```


---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:57 on 2024-07-12 16:05_

Maybe instead of a `invoked_via_uvx` bool we should have an enum that implements `Display`? Seems slightly better to have a type. Doesn't feel critical though, I'm not sure what else we'd do with it.

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2093 on 2024-07-12 16:07_

Instead of adding a flag, could you make use of the `Uvx` variant at https://github.com/astral-sh/uv/blob/c14de2a92a4a4797be56eca79bbe644c762aab7b/crates/uv/src/lib.rs#L599

We should just be able to tell if `uvx` was invoked from there with `matches!`

---

_@zanieb reviewed on 2024-07-12 16:07_

---

_Label `cli` added by @zanieb on 2024-07-12 16:07_

---

_Label `preview` added by @zanieb on 2024-07-12 16:07_

---

_@blueraft reviewed on 2024-07-12 16:37_

---

_Review comment by @blueraft on `crates/uv-cli/src/lib.rs`:2093 on 2024-07-12 16:37_

Missed that before, done 😅

---

_@charliermarsh reviewed on 2024-07-12 16:43_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/run.rs`:57 on 2024-07-12 16:43_

Yeah enum with type seems nice :)

---

_Comment by @charliermarsh on 2024-07-12 16:43_

Thanks so much, that was so fast!

---

_@zanieb reviewed on 2024-07-12 16:45_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2093 on 2024-07-12 16:45_

I took this a bit further in https://github.com/astral-sh/uv/pull/5014/commits/3426d0ad4e4a68dc68753b38a798d4e4b7f9a856

---

_@zanieb reviewed on 2024-07-12 16:48_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:57 on 2024-07-12 16:48_

@blueraft interested in adding a `ToolRunCommand` enum and using that?

---

_@blueraft reviewed on 2024-07-12 16:48_

---

_Review comment by @blueraft on `crates/uv/src/commands/tool/run.rs`:57 on 2024-07-12 16:48_

Done

---

_@blueraft reviewed on 2024-07-12 16:51_

---

_Review comment by @blueraft on `crates/uv-cli/src/lib.rs`:2093 on 2024-07-12 16:51_

Thanks! very cool, didn't know about that syntax/feature before!

---

_@zanieb reviewed on 2024-07-12 17:07_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:57 on 2024-07-12 17:07_

Thanks!

---

_Merged by @zanieb on 2024-07-12 17:17_

---

_Closed by @zanieb on 2024-07-12 17:17_

---
