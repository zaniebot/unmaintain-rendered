---
number: 16434
title: "`requires-python` comparison in `uv python find` rejects 3.13t"
type: issue
state: open
author: lmmx
labels:
  - bug
assignees: []
created_at: 2025-10-24T09:02:14Z
updated_at: 2025-12-17T11:55:44Z
url: https://github.com/astral-sh/uv/issues/16434
synced_at: 2026-01-10T01:26:06Z
---

# `requires-python` comparison in `uv python find` rejects 3.13t

---

_Issue opened by @lmmx on 2025-10-24 09:02_

### Summary

- Opening this as a more specific version of #16428 which should be easier to repro

The bug appears to stem from the handling of `PythonVariant::Default` [in `uv-python`'s discovery module](https://github.com/astral-sh/uv/blob/17181fef07326707ca6921aba28d1dbdd1e6ce83/crates/uv-python/src/discovery.rs#L1669-L1689) rejecting interpreters where `interpreter.gil_disabled()` is true (free-threaded builds like 3.13t), even though the version number satisfies the `requires-python` range.

The skip is within a parent routine that emits this line

> DEBUG Using Python request `>=3.12` from `requires-python` metadata

I think it ought be considered a bug if `uv python find` accepts 3.13t but not if `requires-python` (with a _version_ specifier) rejects based on the _variant_ specifier. That is, the behaviour is not rejecting 3.13t outright as experimental but only when filtered as being above some lower version (which is surely illogical).

## Solution: treat requires-python parsing differently?

I was initially not sure whether there is anything to be actioned here as the comment says it is intentional, but on reading more closely I supect that the comment is referring to the separate scenario of _creating_ a Python venv rather than selecting from available ones?

https://github.com/astral-sh/uv/blob/17181fef07326707ca6921aba28d1dbdd1e6ce83/crates/uv-python/src/discovery.rs#L1680-L1681

I would defer to @zanieb's judgement (thanks for initial triage!).

I don't see a way to opt in here though is the problem for me, to say that free-threading is accepted

There is no `uv python find` flag that would let me "opt in" to free threading explicitly (it is in CI and just being picked up automatically from the uv python version env var after the setup-uv action creates it from the matrix string "3.13t")

I asked Claude if the comment here means it is intended behaviour and it suggests that this behaviour should not apply when parsing requires-python (as opposed to `--python`), i.e. when a user is trying to get a new version of python, not when checking whether to use or skip the available versions available on the PATH

<details>
<summary>Click to show Claude review of what should be done here</summary>

> Let me re-read that comment more carefully:
> 
> ```rust
> fn matches_interpreter(self, interpreter: &Interpreter) -> bool {
>     match self {
>         Self::Default => {
>             if (interpreter.python_major(), interpreter.python_minor()) >= (3, 14) {
>                 // For Python 3.14+, the free-threaded build is not considered experimental
>                 // and can satisfy the default variant without opt-in
>                 true
>             } else {
>                 // In Python 3.13 and earlier, the free-threaded build is considered
>                 // experimental and requires explicit opt-in
>                 !interpreter.gil_disabled()
>             }
>         }
> ```
> 
> So the code is **intentionally** rejecting free-threaded Python 3.13 without explicit opt-in. The comment says "requires explicit opt-in" for 3.13t.
> 
> But here's the bug: **This policy is being applied to `requires-python` constraints, which is wrong.**
> 
> The issue is that `requires-python = ">=3.12"` gets parsed as a `Range` with `PythonVariant::Default`. This is semantically saying "I need Python >= 3.12 with the default (non-free-threaded) variant."
> 
> But `requires-python` in a `pyproject.toml` is meant to specify the **minimum Python version**, not the variant. It's a **version constraint**, not a **variant constraint**.
> 
> The bug is that when parsing a version range from `requires-python`, it shouldn't be attaching `PythonVariant::Default` to it. Looking at the parsing code:
> 
> ```rust
> fn parse_version_specifiers_request(
>     s: &str,
>     variant: PythonVariant,
> ) -> Result<VersionRequest, Error> {
>     let Ok(specifiers) = VersionSpecifiers::from_str(s) else {
>         return Err(Error::InvalidVersionRequest(s.to_string()));
>     };
>     if specifiers.is_empty() {
>         return Err(Error::InvalidVersionRequest(s.to_string()));
>     }
>     Ok(VersionRequest::Range(specifiers, variant))
> }
> ```
> 
> The `variant` parameter comes from the earlier parsing which defaults to `PythonVariant::Default` when no variant suffix is present.
> 
> **The real bug**: When `requires-python` metadata is used (not a user `--python` flag), the version request should probably use a variant that accepts any variant for version-only constraints. The source of the version request matters - it should distinguish between "user requested Python 3.12 (default)" vs "project requires Python >= 3.12 (any variant)".

</details>

## Demo

### 3.13t

Use `uv init` to create a new project, it will automatically be given a requires-python

```toml
[project]
name = "nogil"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = []
```

Then create a venv with

```sh
uv venv --python=3.13t
```

`uv python find` gives the incorrect location because it rejects the venv python at comparison

<details>
<summary>Click to show verbose uv python find logs</summary>

```
louis 🚶 ~/lab/nogil $ uv python find -vv
DEBUG uv 0.9.2
TRACE Checking shared lock for `/home/louis/.cache/uv` at `/home/louis/.cache/uv/.lock`
DEBUG Acquired shared lock for `/home/louis/.cache/uv`
DEBUG Found project root: `/home/louis/lab/nogil`
DEBUG No workspace root found, using project root
DEBUG No Python version file found in workspace: /home/louis/lab/nogil
DEBUG Using Python request `>=3.12` from `requires-python` metadata
DEBUG Searching for Python >=3.12 in virtual environments, managed installations, or search path
TRACE Found cached interpreter info for Python 3.13.8, skipping query of: .venv/bin/python3
DEBUG Found `cpython-3.13.8+freethreaded-linux-x86_64-gnu` at `/home/louis/lab/nogil/.venv/bin/python3` (virtual environment)
DEBUG Skipping interpreter at `.venv/bin/python3` from virtual environment: does not satisfy request `>=3.12`
DEBUG Searching for managed installations at `/home/louis/.local/share/uv/python`
TRACE Found `ld` path: /lib64/ld-linux-x86-64.so.2
TRACE stdout output from `ld`: ""
TRACE stderr output from `ld`: "/lib64/ld-linux-x86-64.so.2: missing program name\nTry '/lib64/ld-linux-x86-64.so.2 --help' for more information.\n"
TRACE Tried to find musl version by running `"/lib64/ld-linux-x86-64.so.2"`, but failed: Could not find musl version in output of: `/lib64/ld-linux-x86-64.so.2`
TRACE Tried to find libc version from possible symlink at "/lib64/ld-linux-x86-64.so.2", but failed: Failed to find glibc version in the filename of linker: `/lib/x86_64-linux-gnu/ld-linux-x86-64.so.2`
TRACE stdout output from `ld.so --version`: "ld.so (Ubuntu GLIBC 2.35-0ubuntu3.8) stable release version 2.35.\nCopyright (C) 2022 Free Software Foundation, Inc.\nThis is free software; see the source for copying conditions.\nThere is NO warranty; not even for MERCHANTABILITY or FITNESS FOR A\nPARTICULAR PURPOSE.\n"
TRACE Found manylinux 2.35 in stdout of ld.so version
DEBUG Found managed installation `cpython-3.13.8+freethreaded-linux-x86_64-gnu`
TRACE Found cached interpreter info for Python 3.13.8, skipping query of: /home/louis/.local/share/uv/python/cpython-3.13.8+freethreaded-linux-x86_64-gnu/bin/python3.13t
DEBUG Found `cpython-3.13.8+freethreaded-linux-x86_64-gnu` at `/home/louis/.local/share/uv/python/cpython-3.13.8+freethreaded-linux-x86_64-gnu/bin/python3.13t` (managed installations)
DEBUG Skipping interpreter at `/home/louis/.local/share/uv/python/cpython-3.13.8+freethreaded-linux-x86_64-gnu/bin/python3.13t` from managed installations: does not satisfy request `>=3.12`
DEBUG Found managed installation `cpython-3.12.6-linux-x86_64-gnu`
TRACE Found cached interpreter info for Python 3.12.6, skipping query of: /home/louis/.local/share/uv/python/cpython-3.12.6-linux-x86_64-gnu/bin/python3.12
DEBUG Found `cpython-3.12.6-linux-x86_64-gnu` at `/home/louis/.local/share/uv/python/cpython-3.12.6-linux-x86_64-gnu/bin/python3.12` (managed installations)
/home/louis/.local/share/uv/python/cpython-3.12.6-linux-x86_64-gnu/bin/python3.12
DEBUG Released lock at `/home/louis/.cache/uv/.lock`
```

</details>

> DEBUG Skipping interpreter at `.venv/bin/python3` from virtual environment: does not satisfy request `>=3.12`

> DEBUG Skipping interpreter at `/home/louis/.local/share/uv/python/cpython-3.13.8+freethreaded-linux-x86_64-gnu/bin/python3.13t` from managed installations: does not satisfy request `>=3.12`

If you make one without a `requires-python` key

```toml
[project]
name = "nogil2"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
dependencies = []
```

and again `uv venv --python=3.13t` and `uv python find` you will now get it

```sh
/home/louis/lab/nogil2/.venv/bin/python3
```

<details>
<summary>Click to show verbose uv python find logs</summary>

```
DEBUG uv 0.9.2
TRACE Checking shared lock for `/home/louis/.cache/uv` at `/home/louis/.cache/uv/.lock`
DEBUG Acquired shared lock for `/home/louis/.cache/uv`
DEBUG Found project root: `/home/louis/lab/nogil2`
DEBUG No workspace root found, using project root
DEBUG No Python version file found in workspace: /home/louis/lab/nogil2
DEBUG Searching for default Python interpreter in virtual environments, managed installations, or search path
TRACE Found cached interpreter info for Python 3.13.8, skipping query of: .venv/bin/python3
DEBUG Found `cpython-3.13.8+freethreaded-linux-x86_64-gnu` at `/home/louis/lab/nogil2/.venv/bin/python3` (virtual environment)
/home/louis/lab/nogil2/.venv/bin/python3
DEBUG Released lock at `/home/louis/.cache/uv/.lock`
```

</details>

### 3.14t

**EDIT** unable to repro, going back to CI repro case to try to isolate what caused that

I also see this on 3.14t and include the logs here for completeness (and since the code appears to distinguish the 2 scenarios, treating 3.14t as no longer experimental)

<details>
<summary>Click to show verbose logs for uv python find</summary>

First with no requires-python:

```toml
[project]
name = "nogil3"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
dependencies = []
```

Without requires-python it is found OK

```sh
louis 🚶 ~/lab/nogil3 $ uv venv --python=3.14t
Using CPython 3.14.0
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
louis 🚶 ~/lab/nogil3 $ uv python find
/home/louis/lab/nogil3/.venv/bin/python3
```

With requires-python added it is... also found

```toml
[project]
name = "nogil3"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
dependencies = []
requires-python = ">=3.12"
```

```
louis 🚶 ~/lab/nogil3 $ uv venv --python=3.14t
Using CPython 3.14.0
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
louis 🚶 ~/lab/nogil3 $ uv python find
/home/louis/lab/nogil3/.venv/bin/python3
```

</details>

### Platform

Linux Mint (seen on CI with ubuntu-latest too)

### Version

0.9.2

### Python version

3.13t (also seen on 3.14t)

## Cause

The error about skipping the interpreter is one of 2 files and the part about "does not satisfy request" is unique to `crates/uv-python/src/discovery.rs` so it must be down to the logic there.

<details>
<summary>Click to show AI summary</summary>

> Note: Claude generated this root cause dissection, I'm reviewing it myself now but posting with full disclosure! I think it gets the wrong idea, I think the comments say quite clearly this is deliberately this way so as discussed above there is nothing to do about this (but I defer to your judgement)

The cause is how `VersionRequest` matches against interpreters with the `PythonVariant` handling.

1. When `requires-python = ">=3.12"` is parsed, it becomes a `VersionRequest::Range` with `PythonVariant::Default`
2. The code has a `matches_interpreter` method that checks if an interpreter satisfies the version request
3. For `Range` variants, it calls `variant.matches_interpreter(interpreter)` 

Here's the bug - in the `PythonVariant::matches_interpreter` method:

https://github.com/astral-sh/uv/blob/17181fef07326707ca6921aba28d1dbdd1e6ce83/crates/uv-python/src/discovery.rs#L1669-L1689

- `requires-python = ">=3.12"` creates a `Range` with `PythonVariant::Default`.
- For Python 3.13t, `interpreter.gil_disabled()` returns `true`, so the condition `!interpreter.gil_disabled()` returns `false`, causing the match to fail.

I suspect this is the design flaw: the code treats free-threaded Python 3.13 as requiring explicit opt-in and rejects it when matching against `PythonVariant::Default`, even though the version number itself (`3.13.8`) clearly satisfies `>=3.12`.

The problem is then compounded by this logic in `matches_interpreter`:

```rust
Self::Range(specifiers, variant) => {
    // version check logic...
    specifiers.contains(&version) && variant.matches_interpreter(interpreter)
}
```

Even though `3.13.8` satisfies `>=3.12` in the specifiers check, the `&& variant.matches_interpreter(interpreter)` part fails because the variant is `Default` and the interpreter is free-threaded.

**The fix should be**: When parsing `requires-python` without explicit variant markers (like `t` suffix), it should either:
1. Use `PythonVariant::Any` instead of `PythonVariant::Default` for range specifiers, OR  
2. Modify `PythonVariant::Default::matches_interpreter` to accept free-threaded builds when the version satisfies the requirement (treating the variant as a build detail, not a version constraint)

The second approach aligns better with PEP 440's treatment of local version identifiers - they shouldn't affect version ordering or specifier satisfaction.

</details>

---

_Label `bug` added by @lmmx on 2025-10-24 09:02_

---

_Referenced in [astral-sh/uv#16428](../../astral-sh/uv/issues/16428.md) on 2025-10-24 09:02_

---

_Renamed from "`requires-python` comparison in `uv python find` rejects free-threaded CPython versions" to "`requires-python` comparison in `uv python find` rejects 3.13t" by @lmmx on 2025-10-24 09:27_

---

_Comment by @thqq479 on 2025-10-30 03:05_

same as me 
Skipping interpreter at `/usr/local/bin/python3.13t` from search path: does not satisfy request `>=3.11`

---

_Referenced in [astral-sh/uv#16507](../../astral-sh/uv/issues/16507.md) on 2025-10-30 13:47_

---

_Comment by @thqq479 on 2025-10-30 13:52_

So, how should we solve this problem? Do you have any suggestions? @charliermarsh @lmmx 

---

_Comment by @lmmx on 2025-10-30 14:01_

The relevant reviewer here is @zanieb to decide on as far as I know. I did say "no rush" because I worked around it by hardcoding .venv/bin/python in my CI workflow rather than `$(uv python find)`. I would still like to see it supported.

The fix would be to not reject the 3.13t variant **in version comparisons** for venv selection, and continue to reject in venv creation.

Sorry it has left my mental cache, please refer to thread above

> the scenario of creating a Python venv rather than selecting between them

The comparison should not reject 3.13t when looking for a Python 3.X (if we already made a 3.13t venv it should be an accepted choice) but when creating a new venv it is fine to not pick it unless explicitly specified

---

_Comment by @konstin on 2025-12-16 13:38_

Do I read the original issue correctly, that this works with 3.14t? In that case I think the easiest solution is updating to 3.14t, which unlike 3.13t, doesn't treat the freethreading builds as experimental. The experimental label on it made things a bit odd, because the build does exist, but it also shouldn't be used in regular settings, which the stabilization in 3.14 resolves.

---

_Comment by @lmmx on 2025-12-17 11:54_

No it wasn't that I was looking for a free-threaded Python version, it's that I wanted to test on a matrix of [3,12,3.13,3.14] + [with/without FT] and because `uv python find` conflates "version not suitable for venv creation" with "version not suitable for venv selection" then there is the incorrect behaviour in the situation where you deliberately opted in to download Python 3.13t but then cannot use it with `uv python find` (it silently punts on 3.13t and gives you instead the base Python)

- When I say my workaround was "hardcoding" I meant I had to just point to `.venv/bin/python` as I recall.

---
