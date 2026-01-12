```yaml
number: 5075
title: Rework reformatting in PyProjectTomlMut to respect original indentation
type: pull_request
state: merged
author: flyaroundme
labels: []
assignees: []
merged: true
base: main
head: uv_add_pyproject_toml_indentation
created_at: 2024-07-15T14:57:37Z
updated_at: 2024-07-15T23:13:23Z
url: https://github.com/astral-sh/uv/pull/5075
synced_at: 2026-01-12T16:06:37Z
```

# Rework reformatting in PyProjectTomlMut to respect original indentation

---

_@flyaroundme_

## Summary

So this PR introduces change to how `Array` of dependencies representation is reformatted while `PyProjectTomlMut` is manipulated. These changes are here for it to respect the original indentation.
Closes https://github.com/astral-sh/uv/issues/5009

## Test Plan
Using `pyproject.toml` like
```
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = [
  "requests"
]
```
Executed 
```
$ uv add httpx
```
And expected in `pyproject.toml`
```
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = [
  "requests",
  "httpx",
]
```
Preserving original indentation

---

_Review requested from @ibraheemdev by @zanieb on 2024-07-15 15:46_

---

_@ibraheemdev reviewed on 2024-07-15 16:36_

Thanks for doing this!

Can we add a test to ensure this is working as expected? You can see https://github.com/astral-sh/uv/blob/main/crates/uv/tests/edit.rs#L14 for an example (you can probably omit the part checking `uv.lock`).

---

_Comment by @flyaroundme on 2024-07-15 16:40_

> Thanks for doing this!
> 
> Can we add a test to ensure this is working as expected? You can see https://github.com/astral-sh/uv/blob/main/crates/uv/tests/edit.rs#L14 for an example (you can probably omit the part checking `uv.lock`).

Sure, will do

---

_Assigned to @ibraheemdev by @zanieb on 2024-07-15 16:50_

---

_Review requested from @ibraheemdev by @flyaroundme on 2024-07-15 18:36_

---

_Comment by @flyaroundme on 2024-07-15 19:26_

Updated, please re-review 🙏

---

_Review comment by @ibraheemdev on `crates/uv-distribution/src/pyproject_mut.rs`:437 on 2024-07-15 21:41_

Hmm I'm a little unclear how this works if the line doesn't have a comment?

---

_@ibraheemdev reviewed on 2024-07-15 21:41_

---

_@flyaroundme reviewed on 2024-07-15 22:46_

---

_Review comment by @flyaroundme on `crates/uv-distribution/src/pyproject_mut.rs`:437 on 2024-07-15 22:46_

If the `decor.prefix()` does not have a comment this whole expression will return the `String` that is in `decor.prefix()` trimmed by `"\n"` from the left

---

_@flyaroundme reviewed on 2024-07-15 22:55_

---

_Review comment by @flyaroundme on `crates/uv-distribution/src/pyproject_mut.rs`:437 on 2024-07-15 22:55_

If you're asking about this particular line
```
.map(|s| s.split('#').next().unwrap_or("").to_string())
```
in case there is no '#' in the string it well return the whole string

---

_@ibraheemdev reviewed on 2024-07-15 23:13_

---

_Review comment by @ibraheemdev on `crates/uv-distribution/src/pyproject_mut.rs`:437 on 2024-07-15 23:13_

Ah that makes sense.

---

_@ibraheemdev approved on 2024-07-15 23:13_

Nice work!

---

_Merged by @ibraheemdev on 2024-07-15 23:13_

---

_Closed by @ibraheemdev on 2024-07-15 23:13_

---
