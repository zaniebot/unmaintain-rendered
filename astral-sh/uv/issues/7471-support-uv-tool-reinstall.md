---
number: 7471
title: Support uv tool reinstall
type: issue
state: closed
author: gaborbernat
labels:
  - enhancement
  - uv tool
assignees: []
created_at: 2024-09-17T16:45:59Z
updated_at: 2024-09-25T22:50:59Z
url: https://github.com/astral-sh/uv/issues/7471
synced_at: 2026-01-10T01:24:15Z
---

# Support uv tool reinstall

---

_Issue opened by @gaborbernat on 2024-09-17 16:45_

When upgrading Python interpreters, it would be beneficial to allow upgrading all tools to a different Python. Something like:

```
uv tool reinstall --python 3.13
```

---

_Comment by @zanieb on 2024-09-17 16:51_

Related to https://github.com/astral-sh/uv/issues/7320

I'm a bit hesitant to add a `reinstall` command. I wonder if we could cover this in `uv tool upgrade`?

---

_Label `enhancement` added by @zanieb on 2024-09-17 16:51_

---

_Label `uv tool` added by @zanieb on 2024-09-17 16:51_

---

_Comment by @gaborbernat on 2024-09-17 16:54_

It would be an option:

`uv tool upgrade --python 3.13` could also work 👍 

---

_Comment by @tfsingh on 2024-09-18 07:25_

Working on this, have an initial (rough draft) [here](https://github.com/astral-sh/uv/compare/main...tfsingh:uv:tfsingh/tool-upgrade-python?expand=1)!

Still needs formal testing (but seems to be working after installing a tool with --python 3.12, upgrading with --python 3.13, and verifying the change in uv/tools/tool's pyvenv.cfg)

---

_Referenced in [astral-sh/uv#7605](../../astral-sh/uv/pulls/7605.md) on 2024-09-20 22:08_

---

_Closed by @charliermarsh on 2024-09-25 17:40_

---

_Closed by @charliermarsh on 2024-09-25 17:40_

---

_Comment by @daviewales on 2024-09-25 22:50_

In the PR above, @tfsingh notes:

> Upgrading with --python also upgrades the package itself

This makes sense for the `upgrade` command, as it is normally used to upgrade the version of packages.

However, I think this suggests that a `reinstall` command may still be needed, because it would allow changing the Python version, while keeping all other options unchanged.

See the pipx [reinstall](https://pipx.pypa.io/stable/docs/#pipx-reinstall) and [reinstall-all](https://pipx.pypa.io/stable/docs/#pipx-reinstall-all) commands.

---
