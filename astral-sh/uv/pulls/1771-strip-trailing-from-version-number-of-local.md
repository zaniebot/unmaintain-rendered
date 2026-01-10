```yaml
number: 1771
title: "strip trailing `+` from version number of local Python builds"
type: pull_request
state: merged
author: BurntSushi
labels:
  - bug
assignees: []
merged: true
base: main
head: ag/fix-1357
created_at: 2024-02-20T17:35:51Z
updated_at: 2024-02-23T12:30:11Z
url: https://github.com/astral-sh/uv/pull/1771
synced_at: 2026-01-10T14:54:43Z
```

# strip trailing `+` from version number of local Python builds

---

_Pull request opened by @BurntSushi on 2024-02-20 17:35_

(This PR message is mostly copied from the comment in the code.)

For local builds of Python, at time of writing, the version numbers end with
a `+`. This makes the version non-PEP-440 compatible since a `+` indicates
the start of a local segment which must be non-empty. Thus, `uv` chokes on it
and [spits out an error][1] when trying to create a venv using a "local" build
of Python. Arguably, the right fix for this is for [CPython to use a PEP-440
compatible version number][2].

However, as a work-around for now, [as suggested by pradyunsg][3] as one
possible direction forward, we strip the `+`.

This fix does unfortunately mean that one [cannot specify a Python version
constraint that specifically selects a local version][4]. But at the time of
writing, it seems reasonable to block such functionality on this being fixed
upstream (in some way).

Another alternative would be to treat such invalid versions as strings (which
is what PEP-508 suggests), but this leads to undesirable behavior in this
case. For example, let's say you have a Python constraint of `>=3.9.1` and
a local build of Python with a version `3.11.1+`. Using string comparisons
would mean the constraint wouldn't be satisfied:

    >>> "3.9.1" < "3.11.1+"
    False

So in the end, we just strip the trailing `+`, as was done in the days of old
for [legacy version numbers][5].

I tested this fix by manually confirming that

    uv venv --python local/python

failed before it and succeeded after it.

Fixes #1357

[1]: https://github.com/astral-sh/uv/issues/1357
[2]: https://github.com/python/cpython/issues/99968
[3]: https://github.com/pypa/packaging/issues/678#issuecomment-1436033646
[4]: https://github.com/astral-sh/uv/issues/1357#issuecomment-1947645243
[5]: https://github.com/pypa/packaging/blob/085ff41692b687ae5b0772a55615b69a5b677be9/packaging/version.py#L168-L193


---

_Review requested from @konstin by @BurntSushi on 2024-02-20 17:35_

---

_Review requested from @zanieb by @BurntSushi on 2024-02-20 17:36_

---

_@konstin approved on 2024-02-20 17:42_

---

_Merged by @BurntSushi on 2024-02-20 17:57_

---

_Closed by @BurntSushi on 2024-02-20 17:57_

---

_Branch deleted on 2024-02-20 17:57_

---

_Label `bug` added by @BurntSushi on 2024-02-23 12:30_

---
