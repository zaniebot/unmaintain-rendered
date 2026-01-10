```yaml
number: 4735
title: "Change \"toolchain\" to \"python\""
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zb/toolchain-python
created_at: 2024-07-02T18:57:50Z
updated_at: 2025-10-26T15:51:25Z
url: https://github.com/astral-sh/uv/pull/4735
synced_at: 2026-01-10T06:36:15Z
```

# Change "toolchain" to "python"

---

_Pull request opened by @zanieb on 2024-07-02 18:57_

Whew this is a lot.

The user-facing changes are:

- `uv toolchain` to `uv python` e.g. `uv python find`, `uv python install`, ...
- `UV_TOOLCHAIN_DIR` to` UV_PYTHON_INSTALL_DIR`
- `<UV_STATE_DIR>/toolchains` to `<UV_STATE_DIR>/python` (with [automatic migration](https://github.com/astral-sh/uv/pull/4735/files#r1663029330))
- User-facing messages no longer refer to toolchains, instead using "Python", "Python versions" or "Python installations"

The internal changes are:

- `uv-toolchain` crate to `uv-python`
- `Toolchain` no longer referenced in type names
- Dropped unused `SystemPython` type (previously replaced)
- Clarified the type names for "managed Python installations"
- (more little things)

---

_Label `preview` added by @zanieb on 2024-07-02 18:57_

---

_@zanieb reviewed on 2024-07-02 19:05_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:116 on 2024-07-02 19:05_

This blurb is notable

---

_Marked ready for review by @zanieb on 2024-07-02 19:28_

---

_Comment by @zanieb on 2024-07-02 19:29_

I'll give this diff a careful read through as well.

---

_@charliermarsh approved on 2024-07-02 19:31_

---

_Comment by @charliermarsh on 2024-07-02 19:32_

My only complaint is that `uv python` feels like I'd go `uv python foo.py` (to run Python itself), but I don't have a better suggestion.


---

_Comment by @zanieb on 2024-07-02 19:39_

Yeah I agree that's a little confusing and my primary hesitation. What if we provide a `uv python run` command though with an optional `python` shim on the PATH that invokes it?

---

_Comment by @samypr100 on 2024-07-02 22:01_

Curious, would there be a different short-hand for this similar to uvx? Say `uvp` or `uv py`

---

_Comment by @zanieb on 2024-07-02 22:05_

I think similar to `uvx` mapping to `uv tool run` (but a little different in implementation) we'd allow opt-in installation of `python` (or another name) that maps to `uv python run`.

---

_Merged by @zanieb on 2024-07-03 12:44_

---

_Closed by @zanieb on 2024-07-03 12:44_

---

_Branch deleted on 2024-07-03 12:44_

---

_@HACK55515 reviewed on 2025-10-26 11:57_

-_-

---
