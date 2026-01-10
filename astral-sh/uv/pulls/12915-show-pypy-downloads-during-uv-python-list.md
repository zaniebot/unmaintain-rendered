```yaml
number: 12915
title: "Show PyPy downloads during `uv python list`"
type: pull_request
state: merged
author: alexprengere
labels:
  - bug
assignees: []
merged: true
base: main
head: fix_issue_12914
created_at: 2025-04-16T09:15:57Z
updated_at: 2025-04-17T21:19:12Z
url: https://github.com/astral-sh/uv/pull/12915
synced_at: 2026-01-10T11:10:40Z
```

# Show PyPy downloads during `uv python list`

---

_Pull request opened by @alexprengere on 2025-04-16 09:15_

Fixes #12914.
When `PythonDownloadRequest` does not have the `implementation` set, do not set it to CPython when calling `fill`, otherwise only CPython interpreters are shown when listing interpreters available for download, with `uv python list`.

---

_Review requested from @zanieb by @konstin on 2025-04-16 09:17_

---

_Comment by @zanieb on 2025-04-16 14:06_

Thanks for the pull request!

Hm, but I think we _do_ need to fill CPython as the default somewhere, right? I didn't look into this in depth yet, but we have the `Any` and `Default` request variants for this purpose. Can we leverage those here?

---

_Comment by @alexprengere on 2025-04-17 08:09_

From what I can tell, using `fill()` is a bit wrong in that part of the code, because you do not want fill in CPython as default for *this* call site. There are other call sites for `fill()`, which caused the CI issues:

* [crates/uv/src/commands/python/install.rs#L55](https://github.com/astral-sh/uv/blob/main/crates/uv/src/commands/python/install.rs#L55)
* [crates/uv-python/src/installation.rs#L124](https://github.com/astral-sh/uv/blob/main/crates/uv-python/src/installation.rs#L124)

I believe the usage of `fill()` is correct for those (filling CPython is correct there).
So I created `fill_platform()`, that is only used in the uv-python-list logic.

I see @j178 added tests in a separate PR #12931, if that one is merged first I can rebase and update.

---

_@zanieb approved on 2025-04-17 13:55_

Makes sense, thank you!

---

_Comment by @zanieb on 2025-04-17 13:56_

Separately from the implementation / regression, I sort of wonder if we want to be showing all those PyPy interpreters by default? Most users don't need them.

We do already show them if you request `pypy`

```
❯ uv python list pypy
pypy-3.11.11-macos-aarch64-none    <download available>
pypy-3.10.16-macos-aarch64-none    <download available>
pypy-3.10.14-macos-aarch64-none    /opt/homebrew/bin/pypy3 -> ../Cellar/pypy3.10/7.3.17_1/bin/pypy3
pypy-3.9.19-macos-aarch64-none     <download available>
pypy-3.8.16-macos-aarch64-none     <download available>
```

Maybe this should just be a documentation update?

---

_Comment by @alexprengere on 2025-04-17 14:50_

The reason I looked this up in the first place was because a someone told me "I want to use uv, but uv does not support PyPy".
They were running `uv python list`, and lots of CPython was shown as "download available", but no PyPy.
I work in data science where the use of PyPy is pretty frequent for performance reasons on long running jobs (perhaps to a more general audience, people will care less about this).

---

_Renamed from "Do not set PythonDownloadRequest implementation to CPython when calli…" to "Show PyPy downloads during `uv python list`" by @zanieb on 2025-04-17 16:58_

---

_Comment by @zanieb on 2025-04-17 16:59_

Alright, let's consider that separately.

---

_Label `bug` added by @zanieb on 2025-04-17 16:59_

---

_Merged by @zanieb on 2025-04-17 16:59_

---

_Closed by @zanieb on 2025-04-17 16:59_

---

_Branch deleted on 2025-04-17 21:19_

---
