```yaml
number: 9344
title: "Combine `sys_platform` and `platform_system` in marker algebra"
type: pull_request
state: closed
author: charliermarsh
labels:
  - enhancement
  - performance
assignees: []
base: main
head: charlie/lower-iii
created_at: 2024-11-22T02:27:57Z
updated_at: 2024-12-07T01:44:16Z
url: https://github.com/astral-sh/uv/pull/9344
synced_at: 2026-01-10T12:00:00Z
```

# Combine `sys_platform` and `platform_system` in marker algebra

---

_Pull request opened by @charliermarsh on 2024-11-22 02:27_

## Summary

This PR adds a representation to the marker algebra to capture "known pairs" of `sys_platform` and `platform_system` values.

For example, we want to be able to detect that `sys_platform == 'win32'` and `platform_system == 'Darwin'` are disjoint.

There is some risk here, in that if there are platforms for which, e.g., `sys_platform == 'win32` but `platform_system` is not `Windows`, we'll get things wrong. I'm trying to weigh whether that's a real risk.

In theory, we could _maybe_ also include `os.name` here?

Closes #7760.
Closes #9275.


---

_Review requested from @konstin by @charliermarsh on 2024-11-22 02:28_

---

_Review requested from @ibraheemdev by @charliermarsh on 2024-11-22 02:28_

---

_Label `enhancement` added by @charliermarsh on 2024-11-22 02:28_

---

_Label `performance` added by @charliermarsh on 2024-11-22 02:28_

---

_Comment by @Vinno97 on 2024-11-22 13:27_

> In theory, we could maybe also include os.name here?

Yes. I was just about to comment on #9275  about this. 

For example, `jupyter-server` has a dependency on `pywinpty` for `os_name == 'nt'`. If want linux-only lock-file, a full dependency string would then have to be: `platform_system == 'Linux' and sys_platform == 'linux' and os_name == 'posix'`

---

_Comment by @charliermarsh on 2024-11-22 14:14_

Yeah. I think it's a bit more challenging because there isn't a 1-to-1 mapping between `os.name` and `sys.platform` for Linux and macOS (they both use `posix`).

---

_Comment by @charliermarsh on 2024-11-22 14:17_

(Not impossible but different than what I've encoded here.)

---

_Comment by @Vinno97 on 2024-11-22 15:17_

Since `os_name == 'posix'` could also be true for BSD and other more _esoteric_ POSIX operating systems, perhaps `os_name == ' posix'`  should entail `os_name != 'nt'`, with its subsequent `platform_system` and `sys_platform` restrictions?


---

_Comment by @charliermarsh on 2024-11-23 00:27_

On reflection, what I have here is not correct. It fails this test (i.e., it thinks these are disjoint):

```rust
assert!(!is_disjoint(
    "sys_platform == 'cygwin'",
    "platform_system == 'Java'"
));
```

---

_Comment by @charliermarsh on 2024-11-23 01:04_

Somewhat losing confidence here because it turns out Android also returns `"Linux"` for `platform_system`? Despite `sys_platform` returning `"android"`.

---

_Comment by @Vinno97 on 2024-11-25 09:33_

It seems like Poetry has gone though quite the rabbit hole for this themselves: https://github.com/python-poetry/poetry/issues/5192. But even after quite some pull requests, they closed the isue with this comment:

> Closing since there haven't been much improvements/fixes anymore in the last time and I can't come up with a good solution for merging sys_platform and platform_system. - https://github.com/python-poetry/poetry/issues/5192#issuecomment-1229432876

It appears this may be an unresolvable issue, though perhaps it's feasible to just cull known impossible environments? I.e. we know that `platform_system=='Linux'`  will never occur together with `sys_platform=='win32'`  or `sys_platform=='darwin'`.

---

_Comment by @charliermarsh on 2024-11-25 13:46_

I think it's absolutely possible to solve in our marker system, I just don't have the right solution here.

---

_Comment by @konstin on 2024-11-26 11:42_

This is a tough one, my intuition is to encode know mismatch rather than known pairs. There's a tendency to subdivide platforms to accommodate new developments (the development of rust target triples is a good reference since it's more upstreamed than the python build vars), so i would encode tier 1 and tier 2 pairs that never match, such as darwin `sys_platform` and Windows in `platform_system`.

---

_Comment by @Vinno97 on 2024-11-26 11:55_

@konstin that's a good point and probably catches the majority of the standard cases with a smaller chance if breaking future cases

---

_Comment by @charliermarsh on 2024-11-26 14:02_

Yes that's also what I want to do. Here's a new idea: we encode all mutually exclusive pairs, and for each pair (A, B), we add `and (not A or not B)` to each expression. In practice, it's probably easier to have a method like `is_impossible` alongside `is_false` that just does a single operation to check this for all pairs at once.

---

_Comment by @charliermarsh on 2024-12-07 01:44_

Closing in favor of https://github.com/astral-sh/uv/pull/9444.

---

_Closed by @charliermarsh on 2024-12-07 01:44_

---
