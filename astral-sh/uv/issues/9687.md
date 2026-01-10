```yaml
number: 9687
title: "`uv pip install ./local/git/repo` locks the git repository, `pip install` does not"
type: issue
state: closed
author: Luthaf
labels:
  - bug
  - needs-mre
assignees: []
created_at: 2024-12-06T15:33:43Z
updated_at: 2025-11-07T16:29:20Z
url: https://github.com/astral-sh/uv/issues/9687
synced_at: 2026-01-10T03:23:53Z
```

# `uv pip install ./local/git/repo` locks the git repository, `pip install` does not

---

_Issue opened by @Luthaf on 2024-12-06 15:33_

I have some code in my `setup.py` which calls `git update-index --really-refresh` and `git diff-index --quiet HEAD --` to know whether the user is building an unmodified commit or has uncommitted local changes. This is then used to add a local part to the version of the package, for future reference.

This works fine with `pip install`, but it seems that `uv pip install` does more on the git side, and I get the following error from `git`:

```
fatal: Unable to create '<REPO/PATH>/.git/index.lock': File exists.

Another git process seems to be running in this repository, e.g.
an editor opened by 'git commit'. Please make sure all processes
are terminated then try again. If it still fails, a git process
may have crashed in this repository earlier:
remove the file manually to continue.
```

Could it be possible to release the lock before calling `setup.py`? This happens during `Calling setuptools.build_meta.build_wheel("~/.cache/uv/builds-v0/.tmpmZ68kQ", {}, None)`).

This only seems to happen when the repository is in "dirty" state, with some uncommitted changes.

EDIT: this is with uv version 0.5.6, in a `venv` virtualenv with Python 3.12.0 on macOS, and `git --version` is `git version 2.39.3 (Apple Git-146)`.

---

_Comment by @zanieb on 2024-12-06 15:43_

Thanks for the report. It's not obvious to me where this is happening, might take some digging.

---

_Label `bug` added by @zanieb on 2025-01-07 19:27_

---

_Comment by @zanieb on 2025-01-07 19:28_

Is there a simple way to reproduce this?

---

_Label `needs-mre` added by @zanieb on 2025-01-07 19:28_

---

_Comment by @Luthaf on 2025-01-08 09:54_

I am now unable to reproduce this on the original project that prompted it (for reference, this is https://github.com/metatensor/metatensor). Might have been something strange with my setup, sorry about the noise! 

If this happens again I'll try to extract an MRE!

---

_Closed by @Luthaf on 2025-01-08 09:54_

---

_Comment by @HaoZeke on 2025-11-07 16:15_

This is present still, on dirty repos (with unstaged changes).

---

_Comment by @notatallshaw on 2025-11-07 16:29_

@HaoZeke I would recommend opening a new issue with steps to reproduce 

---
