---
number: 12332
title: uv tool list - possiblity to show python versions
type: issue
state: open
author: tkossak
labels:
  - enhancement
assignees: []
created_at: 2025-03-20T08:48:45Z
updated_at: 2025-05-05T06:26:01Z
url: https://github.com/astral-sh/uv/issues/12332
synced_at: 2026-01-07T13:12:18-06:00
---

# uv tool list - possiblity to show python versions

---

_Issue opened by @tkossak on 2025-03-20 08:48_

### Summary

Hi, feature request for `uv tool list` that I'm missing from pipx:  should show (or have option for) which python version was used to install each tool

Thank you.

### Example

Showing python versions would allow us to decide if we can safely remove old python versions or if it's time to upgrade python for specific tool


---

_Label `enhancement` added by @tkossak on 2025-03-20 08:48_

---

_Renamed from "uv tool - possiblity to show python versions and upgrade all packages" to "uv tool - possiblity to show python versions and upgrade/reinstall all packages" by @tkossak on 2025-03-20 08:49_

---

_Renamed from "uv tool - possiblity to show python versions and upgrade/reinstall all packages" to "uv tool - possiblity to show python versions and reinstall all packages" by @tkossak on 2025-03-20 09:22_

---

_Renamed from "uv tool - possiblity to show python versions and reinstall all packages" to "uv tool list - possiblity to show python versions" by @tkossak on 2025-03-20 09:24_

---

_Comment by @mgedmin on 2025-05-05 06:21_

I was very surprised today when a uv tool-installed flake8 started complaining about Python 3.12 syntax in my code, and then discovered that it was using a uv-managed Python 3.10 as the interpreter.  

I had reinstalled all of my tools (with uv tool install --reinstall each one separately, wishing uv had pipx's reinstall-all) after an OS upgrade and expected them to use my OS-default /usr/bin/python3 (which is 3.13), not a uv python-installed old 3.10 from I don't even remember when.  I wanted to check which Python was used by each tool and hoped `uv tool list -v` or `--show-version-specifiers` would tell me, which it didn't.  I'd love a `uv tool list --show-python-versions` or something please.

In the mean time this works as a workaround:

    ls -l ~/.local/share/uv/tools/*/bin/python

---
