```yaml
number: 15961
title: Document support for free-threaded and debug Python versions
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/ft-debug-docs
created_at: 2025-09-20T15:35:21Z
updated_at: 2025-09-22T13:59:42Z
url: https://github.com/astral-sh/uv/pull/15961
synced_at: 2026-01-10T06:36:15Z
```

# Document support for free-threaded and debug Python versions

---

_Pull request opened by @zanieb on 2025-09-20 15:35_

Closes #12707 

---

_Label `documentation` added by @zanieb on 2025-09-20 15:35_

---

_Comment by @zanieb on 2025-09-20 18:08_

cc @geofft 

xref #15756 

---

_Review comment by @geofft on `crates/uv-cli/src/lib.rs`:440 on 2025-09-20 18:36_

Should we call this "ABI flag," for consistency with e.g. `sys.abiflags`?

---

_Review comment by @geofft on `docs/concepts/python-versions.md`:333 on 2025-09-20 18:38_

```suggestion
uv supports discovering and installing [free-threaded](https://py-free-threading.github.io/) Python variants in CPython 3.13+.
```

or `https://docs.python.org/3.14/glossary.html#term-free-threading`

---

_Review comment by @geofft on `docs/concepts/python-versions.md`:340 on 2025-09-20 18:42_

```suggestion
uv supports discovering and installing [debug builds](https://docs.python.org/3.14/using/configure.html#debug-build) of Python, i.e., with debug assertions enabled.
```

Also probably worth adding somewhere -

The debug builds of the managed Python installation also have debugging symbols included, which make it easier to use a C-level debugger.

---

_@geofft approved on 2025-09-20 18:42_

---

_@zanieb reviewed on 2025-09-20 18:46_

---

_Review comment by @zanieb on `docs/concepts/python-versions.md`:333 on 2025-09-20 18:46_

I'd probably rather link to `python.org`, yeah.

---

_@zanieb reviewed on 2025-09-20 18:48_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:440 on 2025-09-20 18:48_

Hm. I'll think about that. I probably don't want to introduce another concept.

---

_@zanieb reviewed on 2025-09-20 18:48_

---

_Review comment by @zanieb on `docs/concepts/python-versions.md`:340 on 2025-09-20 18:48_

I don't want to mix that use-case into here, since we want to enable debug-symbols through another method. Maybe worth having in the meantime though.

---

_@geofft reviewed on 2025-09-20 20:20_

---

_Review comment by @geofft on `docs/concepts/python-versions.md`:340 on 2025-09-20 20:20_

Yeah, I meant this as an "FYI this happens to work if that's what you're looking for, but it isn't the point of the debug build, it's not guaranteed, and it may not apply to unmanaged builds." I think it's worth mentioning with some sort of phrasing to that effect.

---

_@zanieb reviewed on 2025-09-22 13:20_

---

_Review comment by @zanieb on `docs/concepts/python-versions.md`:333 on 2025-09-22 13:20_

```suggestion
uv supports discovering and installing [free-threaded](https://docs.python.org/3.14/glossary.html#term-free-threading) Python variants in CPython 3.13+.
```


---

_@geofft reviewed on 2025-09-22 13:37_

---

_Review comment by @geofft on `docs/concepts/python-versions.md`:345 on 2025-09-22 13:37_

This is less accurate than the phrasing I suggested :) As I understand it, uv will both _discover_ existing `--with-pydebug` builds on the system (e.g. `dnf install python3.14-debug`, which gets you a `/usr/bin/python3.14d`), and _install_ our own managed versions, and in both cases it will only use them if it cannot find a non-debug version. But the lack of stripping of debug symbols only applies to our own debug builds and may not apply to existing discoverable versions. (And it also only applies for now, I would like to do something else about debug symbols in the future.)

---

_@geofft reviewed on 2025-09-22 13:38_

---

_Review comment by @geofft on `crates/uv-cli/src/lib.rs`:440 on 2025-09-22 13:38_

I would argue that this naming is what's already used in the Python docs and "short variant" would be introducing new naming, but I see the argument that we're already using "variant," hm....

---

_@zanieb reviewed on 2025-09-22 13:38_

---

_Review comment by @zanieb on `docs/concepts/python-versions.md`:345 on 2025-09-22 13:38_

These are all great reasons not to say this at all imo, it's too complicated :)

---

_@zanieb reviewed on 2025-09-22 13:40_

---

_Review comment by @zanieb on `docs/concepts/python-versions.md`:345 on 2025-09-22 13:40_

I changed it.

---

_@geofft approved on 2025-09-22 13:45_

---

_@zanieb reviewed on 2025-09-22 13:49_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:440 on 2025-09-22 13:49_

Let's just track this in an issue for now.

---

_Merged by @zanieb on 2025-09-22 13:59_

---

_Closed by @zanieb on 2025-09-22 13:59_

---

_Branch deleted on 2025-09-22 13:59_

---
