---
number: 1697
title: Report uv version in pyvenv.cfg
type: issue
state: closed
author: henryiii
labels:
  - enhancement
  - good first issue
  - virtualenv
assignees: []
created_at: 2024-02-19T15:00:22Z
updated_at: 2024-02-22T19:39:38Z
url: https://github.com/astral-sh/uv/issues/1697
synced_at: 2026-01-07T13:12:16-06:00
---

# Report uv version in pyvenv.cfg

---

_Issue opened by @henryiii on 2024-02-19 15:00_

Since virutalenv 20, it's included a `virtualenv=` field in `pyvenv.cfg` to indicate the version of virtualenv that created the venv. It would be nice to provide a similar `uv=` (suggested) field to report the version of uv used to create the virtualenv. This would also help nox detect which tool created the virtual environment to handle switching more elegantly.

There is a `gourgeist` field, don't know what that is, but if I can be sure that always means `uv`, that would be fine (though knowing the uv version would still be nice). Though from some quick searches, perhaps it would only be there [on UNIX systems](https://github.com/astral-sh/uv/blob/b76efc62a72ed8031aa91ae80bb1b4a159bd3d21/crates/gourgeist/README.md?plain=1#L5)?

---

_Label `enhancement` added by @zanieb on 2024-02-19 15:30_

---

_Label `virtualenv` added by @zanieb on 2024-02-19 15:30_

---

_Comment by @zanieb on 2024-02-19 15:30_

`gourgeist` is the crate by @konstin that actually manages virtual environments. It'd make sense to have it accept a name / version to pass through though and use `uv`.

---

_Label `good first issue` added by @zanieb on 2024-02-19 15:30_

---

_Comment by @henryiii on 2024-02-19 17:53_

Does it live in this repo? Or is it copied from somewhere else?

---

_Comment by @charliermarsh on 2024-02-19 17:54_

It lives in this repo.

---

_Comment by @charliermarsh on 2024-02-19 17:55_

(To avoid confusion: there's a version at https://github.com/konstin/gourgeist but we ended up moving all the crates into this repo so that we could develop them as a monorepo. We'll likely either deprecate https://github.com/konstin/gourgeist and publish from here in the future, or back changes from this repo back out to https://github.com/konstin/gourgeist. But all development should happen on the version of `gourgeist` in `crates`.)

---

_Referenced in [astral-sh/uv#1852](../../astral-sh/uv/pulls/1852.md) on 2024-02-22 03:44_

---

_Closed by @konstin on 2024-02-22 19:39_

---
