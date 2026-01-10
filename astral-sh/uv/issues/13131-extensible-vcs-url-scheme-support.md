```yaml
number: 13131
title: Extensible VCS/URL scheme support.
type: issue
state: closed
author: oh-ok
labels:
  - enhancement
  - wish
assignees: []
created_at: 2025-04-27T13:44:23Z
updated_at: 2025-05-04T09:06:36Z
url: https://github.com/astral-sh/uv/issues/13131
synced_at: 2026-01-10T03:41:47Z
```

# Extensible VCS/URL scheme support.

---

_Issue opened by @oh-ok on 2025-04-27 13:44_

### Summary

Hi all! Thanks for the fantastic work on `ruff` and `uv`.

Currently, PyPA's `pip` supports installing from `git`, `subversion`, `mercurial` and `bazaar`.
`uv` and `cargo` only support dependencies from `git` currently.

For most projects this is fine (since public packages are almost always on PyPI anyway), but it would be nice to have the option of VCS for installing private projects.

The problem with supporting multiple VCS systems is that they are a constantly moving target, and supporting them correctly is a massive increase is scope for `uv`. 

[Looking at `cargo`'s response to adding new VCS systems](https://github.com/rust-lang/cargo/issues/12102#issuecomment-1540686779), I'm wondering if this can be another area where `uv` can innovate by providing a means for developers to extend `uv` themselves with their own URL schemes.

Python already has a solution for delegating responsibility in this way with [entry-points](https://setuptools.pypa.io/en/latest/userguide/entry_point.html#advertising-behavior), so I think `uv` is the perfect project to implement something like this.

In this imaginary system, a Python package could define some hooks as entry-points which `uv tool install` could check for, which would then be delegated to by the main `uv` program when it encounters that URL scheme.

Supporting a new URL scheme would be as simple as creating a Python package for it, and then `uv tool install uv-mercurial` for example.

I'm sure there are many complexities and performance considerations to this, so let me know your thoughts.

### Example

_No response_

---

_Label `enhancement` added by @oh-ok on 2025-04-27 13:44_

---

_Label `wish` added by @charliermarsh on 2025-04-27 15:10_

---

_Comment by @oh-ok on 2025-05-04 09:06_

Closing since there was little interest in this idea

---

_Closed by @oh-ok on 2025-05-04 09:06_

---
