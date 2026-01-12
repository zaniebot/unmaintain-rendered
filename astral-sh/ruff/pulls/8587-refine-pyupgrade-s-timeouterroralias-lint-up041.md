```yaml
number: 8587
title: "refine pyupgrade's TimeoutErrorAlias lint (UP041) to remove false positives"
type: pull_request
state: merged
author: BurntSushi
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: ag/fix-8565
created_at: 2023-11-09T17:46:34Z
updated_at: 2023-11-10T15:15:35Z
url: https://github.com/astral-sh/ruff/pull/8587
synced_at: 2026-01-12T15:55:26Z
```

# refine pyupgrade's TimeoutErrorAlias lint (UP041) to remove false positives

---

_@BurntSushi_

Previously, this lint had its alias detection logic a little
backwards. That is, for Python 3.11+, it would *only* detect
asyncio.TimeoutError as an alias, but it should have also detected
socket.timeout as an alias. And in Python <3.11, it would falsely
detect asyncio.TimeoutError as an alias where it should have only
detected socket.timeout as an alias.

We fix it so that both asyncio.TimeoutError and socket.timeout are
detected as aliases in Python 3.11+, and only socket.timeout is
detected as an alias in Python 3.10.

Fixes #8565

## Test Plan

I tested this by updating the existing snapshot test which had erroneously
asserted that socket.timeout should not be replaced with TimeoutError in Python
3.11+. I also added a new regression test that targets Python 3.10 and ensures
that the suggestion to replace asyncio.TimeoutError with TimeoutError does not
occur.

---

_Review requested from @zanieb by @BurntSushi on 2023-11-09 17:50_

---

_Review requested from @charliermarsh by @BurntSushi on 2023-11-09 17:51_

---

_Comment by @github-actions[bot] on 2023-11-09 18:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @alex-700 on 2023-11-09 23:20_

@BurntSushi hi! Thanks for the PR!

If I understand correctly, you decide to leave linting `asyncio.TimeoutError` even for `py310` (but make the fix unsafe).
IMHO, it looks a bit counterintuitive.
The user, targeted to `py310`, can do nothing with the triggered lint about `asyncio.TimeoutError`, but suppressing.
Should we just do not produce a warning about `asyncio.TimeoutError` in the case of `py310`?

---

_Comment by @charliermarsh on 2023-11-09 23:59_

Ah yeah, I think we may have misdiagnosed in the original issue. We probably shouldn't raise on `asyncio.TimeoutError` in Python 3.10 _at all_. The initial PR actually suggests that this was the intention:

<img width="313" alt="Screen Shot 2023-11-09 at 5 08 56 PM" src="https://github.com/astral-sh/ruff/assets/1309177/02f13677-ed55-450d-b4b9-c30b9e598a65">

But then has this code:

```rust
/// Return `true` if an [`Expr`] is an alias of `TimeoutError`.
fn is_alias(expr: &Expr, semantic: &SemanticModel, target_version: PythonVersion) -> bool {
    semantic.resolve_call_path(expr).is_some_and(|call_path| {
        if target_version >= PythonVersion::Py311 {
            matches!(call_path.as_slice(), [""] | ["asyncio", "TimeoutError"])
        } else {
            matches!(
                call_path.as_slice(),
                [""] | ["asyncio", "TimeoutError"] | ["socket", "timeout"]
            )
        }
    })
}
```

We probably want to return both errors if >= Python 3.11, and only `socket.timeout` error if Python 3.10?


---

_Comment by @alex-700 on 2023-11-10 11:21_

@charliermarsh yes. 
Something like 
```rust
/// Return `true` if an [`Expr`] is an alias of `TimeoutError`.
fn is_alias(expr: &Expr, semantic: &SemanticModel, target_version: PythonVersion) -> bool {
    semantic.resolve_call_path(expr).is_some_and(|call_path| {
        if target_version >= PythonVersion::Py311 {
            matches!(
                call_path.as_slice(),
                [""] | ["asyncio", "TimeoutError"] | ["socket", "timeout"]
            )
        } else {
            matches!(call_path.as_slice(), [""] | ["socket", "timeout"])
        }
    })
}
```
^ to minimize a difference from the current code.

Or maybe something like
```rust
/// Return `true` if an [`Expr`] is an alias of `TimeoutError`.
fn is_alias(expr: &Expr, semantic: &SemanticModel, target_version: PythonVersion) -> bool {
    semantic
        .resolve_call_path(expr)
        .is_some_and(|call_path| match call_path.as_slice() {
            [""] => true,
            ["socket", "timeout"] if target_version >= PythonVersion::Py310 => true,
            ["asyncio", "TimeoutError"] if target_version >= PythonVersion::Py311 => true,
            _ => false,
        })
}
 ```
or (implicitly used the information that target_version >= py310)
```rust
/// Return `true` if an [`Expr`] is an alias of `TimeoutError`.
fn is_alias(expr: &Expr, semantic: &SemanticModel, target_version: PythonVersion) -> bool {
    semantic
        .resolve_call_path(expr)
        .is_some_and(|call_path| {
            matches!(call_path.as_slice(), [""] | ["socket", "timeout"])
            || matches!(call_path.as_slice(), ["asyncio", "TimeoutError"] if target_version >= PythonVersion::Py311)
        })
}
```

P.S. I am actually does not understand why the check for [""] is here 😄 

---

_Comment by @BurntSushi on 2023-11-10 12:38_

Right, the `unsafe` fix on Python 3.10 for `asyncio.TimeoutError` was what I was uncertain about. I structured this PR such that all I have to do is drop the last commit. I fixed the logic as @charliermarsh indicated above in the first commit. :-)

---

_Comment by @BurntSushi on 2023-11-10 12:41_

Okay, I've dropped the last commit and updated the PR description.

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyupgrade/rules/timeout_error_alias.rs`:77 on 2023-11-10 14:41_

We can remove the `[""]` piece from this and the preceding branch -- I think that was just a misunderstanding in the original PR. (It has no effect.)

---

_@charliermarsh approved on 2023-11-10 14:41_

Excellent, thanks!

---

_Label `bug` added by @zanieb on 2023-11-10 14:52_

---

_Label `rule` added by @zanieb on 2023-11-10 14:52_

---

_Merged by @BurntSushi on 2023-11-10 15:15_

---

_Closed by @BurntSushi on 2023-11-10 15:15_

---

_Branch deleted on 2023-11-10 15:15_

---
