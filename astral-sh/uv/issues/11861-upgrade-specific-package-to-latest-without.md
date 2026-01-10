---
number: 11861
title: "upgrade specific package to latest without touching `pyproject.toml` ?"
type: issue
state: closed
author: pchalasani
labels:
  - question
assignees: []
created_at: 2025-02-28T16:56:38Z
updated_at: 2025-02-28T17:37:45Z
url: https://github.com/astral-sh/uv/issues/11861
synced_at: 2026-01-10T01:25:12Z
---

# upgrade specific package to latest without touching `pyproject.toml` ?

---

_Issue opened by @pchalasani on 2025-02-28 16:56_

### Question

Searched the docs/issues and found many similar-sounding things but not quite exactly this:

I just want to:
-  upgrade a dependency `blah` to the latest version from PyPi subject to any constraints in the 
`pyproject.toml`, 
- update `uv.lock`
- update other pkgs as needed (also subject to their constraints of course)
- failout if a consistent upgrade is not possible.

I found these commands but they don't do it:
```
uv sync --help | grep upgrade
  -U, --upgrade                            Allow package upgrades, ignoring pinned versions in any existing output file. Implies `--refresh`
  -P, --upgrade-package <UPGRADE_PACKAGE>  Allow upgrades for a specific package, ignoring pinned versions in any existing output file. Implies
```

Maybe this is documented somewhere but I'm finding it easier to ask here (apologies in advance!)


### Platform

MacOS

### Version

0.5.8

---

_Label `question` added by @pchalasani on 2025-02-28 16:56_

---

_Comment by @zanieb on 2025-02-28 17:17_

`uv lock --upgrade-package blah` (or `uv sync --upgrade-package blah`) should do what you ask. Do you have an example of it not?

---

_Comment by @pchalasani on 2025-02-28 17:37_

> `uv lock --upgrade-package blah` (or `uv sync --upgrade-package blah`) should do what you ask. Do you have an example of it not?

Actually it does work (there was some other issue in my env), and thanks for clarifying, and sorry about the distraction


---

_Closed by @pchalasani on 2025-02-28 17:37_

---
