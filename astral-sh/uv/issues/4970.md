```yaml
number: 4970
title: "`uv python pin` should support writing to a `uv.toml` or `pyproject.toml`"
type: issue
state: open
author: zanieb
labels:
  - needs-design
assignees: []
created_at: 2024-07-10T17:05:57Z
updated_at: 2025-08-12T05:52:08Z
url: https://github.com/astral-sh/uv/issues/4970
synced_at: 2026-01-10T03:32:44Z
```

# `uv python pin` should support writing to a `uv.toml` or `pyproject.toml`

---

_Issue opened by @zanieb on 2024-07-10 17:05_

We need to design the schema for this data. Probably at `uv.python-version` or `uv.python.default-version`.

---

_Label `needs-design` added by @zanieb on 2024-07-10 17:05_

---

_Label `preview` added by @zanieb on 2024-07-10 17:05_

---

_Comment by @T-256 on 2024-07-11 15:06_

Then, should it also be top-level command `uv pin`? since it tries to manage project/workspace directly.

---

_Comment by @zanieb on 2024-07-11 15:20_

That's a reasonable question. I'm not sure. I don't love it at the top-level since it's not obvious what we're pinning.

---

_Comment by @zanieb on 2024-07-24 02:40_

I duped myself, https://github.com/astral-sh/uv/issues/4359 covers reading from the configuration file.

---

_Assigned to @zanieb by @charliermarsh on 2024-07-30 16:13_

---

_Comment by @charliermarsh on 2024-07-30 16:13_

(At least figure out the rough design prior to the release.)

---

_Label `preview` removed by @zanieb on 2024-08-20 18:22_

---

_Comment by @edmorley on 2024-12-11 23:37_

I'm slightly concerned this will lead to more fragmentation of where third party ecosystems (such as Heroku, GHA setup-python, Dependabot, ...) will need to check for Python versions when bootstrapping an environment.

If there was a field in `pyproject.toml` that was guaranteed in the upstream spec to be a single Python version (either `X.Y.Z` or `X.Y`) rather than a multiple major version range, then I'd be fine with the ecosystem consolidating around that, but as-is the existing `pyproject.toml` `requires-python` field isn't suitable for bootstrapping purposes, and so the `.python-version` file is the next best thing available - and pretty widely accepted at this point.

---

_Comment by @zanieb on 2024-12-12 01:31_

> ... as-is the existing pyproject.toml requires-python field isn't suitable for bootstrapping purposes, and so the .python-version file is the next best thing available - and pretty widely accepted at this point.

Agreed. That's why we've delayed implementing this. Unfortunately, we're getting quite a bit of confusion from users. I'm not sure how we'll address that.

---

_Comment by @trondhindenes on 2025-08-12 05:50_

just to opine on this: The number one issue I find when helping fellow python devs un-tangling their botched python dev setups, is the presence of a ".python-version" file in some directory, since it has potential far-reaching effects. If the user happens to have "pyenv" installed, then this file in the current (or parent) dir(s) will replace the default python interpreter, leading to expected commands not being available etc. 

It is therefore problematic for uv to create and/or use this file, as it has side-efects far outside of uv itself (think developer just cding into a dir in their terminal in order to run some command). 
The fact that ".python-version" is "widely acceptable" is the exact reason uv should _not_ go anywhere near it - it should aim to minimize the risk affecting other aspects of a developer's environment. I would ask maintaines to consider at least supporting that an alternate filename like `.uv.python-version` or similar is supported as a fallback if ".python-version" is not found.

---
