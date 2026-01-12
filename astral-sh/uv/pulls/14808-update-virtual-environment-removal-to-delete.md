```yaml
number: 14808
title: "Update virtual environment removal to delete `pyvenv.cfg` last"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/remove-venv
created_at: 2025-07-22T11:21:46Z
updated_at: 2025-07-22T13:13:40Z
url: https://github.com/astral-sh/uv/pull/14808
synced_at: 2026-01-12T16:11:26Z
```

# Update virtual environment removal to delete `pyvenv.cfg` last

---

_@zanieb_

An alternative to https://github.com/astral-sh/uv/pull/14569

This isn't a complete solution to https://github.com/astral-sh/uv/issues/13986, in the sense that it's still "fatal" to `uv sync` if we fail to delete an environment, but I think that's okay — deferring deletion is much more complicated. This at least doesn't break users once the deletion fails. The downside is we'll generally treat this virtual environment is valid, even if we nuked a bunch of it.

Closes https://github.com/astral-sh/uv/issues/13986

---

_Label `bug` added by @zanieb on 2025-07-22 11:21_

---

_Comment by @geofft on 2025-07-22 13:07_

If the motivation here is failing to delete Scripts\python.exe, would it be better to try to delete that one first as opposed to deleting pyvenv.cfg last?

Mostly I worry that Scripts\python.exe + pyvenv.cfg is enough to make a valid virtual environment IIRC (as in Python will start up), but it's a broken one if we've deleted everything else, and _especially_ if there's stuff running in the background that might get weird. If the first thing we do is delete Scripts\python.exe and it fails, then we've left the venv unmodified.

(But this PR still seems like an improvement over the status quo so this is not an objection.)

---

_Comment by @zanieb on 2025-07-22 13:12_

> If the motivation here is failing to delete Scripts\python.exe, would it be better to try to delete that one first as opposed to deleting pyvenv.cfg last?

I think that's reasonable too, I'd probably do both? The thing with deleting `python.exe` is we probably want to use a deferred deletion, like we do for the `uv.exe` so I think the approach is a little different.

Any of these executables could be locked, so we're sort of playing whackamole by deleting things first. I think `python.exe` is a good special-case though.

---

_@charliermarsh approved on 2025-07-22 13:12_

---

_Merged by @zanieb on 2025-07-22 13:13_

---

_Closed by @zanieb on 2025-07-22 13:13_

---

_Branch deleted on 2025-07-22 13:13_

---
