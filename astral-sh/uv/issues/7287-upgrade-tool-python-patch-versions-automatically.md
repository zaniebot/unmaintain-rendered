```yaml
number: 7287
title: Upgrade tool Python patch versions automatically on managed Python upgrade
type: issue
state: closed
author: neutrinoceros
labels:
  - bug
  - uv python
  - uv tool
assignees: []
created_at: 2024-09-11T10:36:33Z
updated_at: 2025-06-20T14:17:14Z
url: https://github.com/astral-sh/uv/issues/7287
synced_at: 2026-01-10T03:32:44Z
```

# Upgrade tool Python patch versions automatically on managed Python upgrade

---

_Issue opened by @neutrinoceros on 2024-09-11 10:36_

Here's a scenario where I have a tool (say, `pre-commit`) installed against a uv-managed python interpreter (say 3.12.5) which also happens to be my only uv-managed python 3.12. Trying to upgrade to Python 3.12.6 results in a broken state where Python 3.12.5 is dropped under my tool.

```shell
$ uv python install 3.12.5 # important: there must be only point version of 3.12 managed by uv after this command
$ uv tool install pre-commit -p 3.12 --python-preference=only-managed
$ uv tool list
pre-commit v3.8.0
- pre-commit
$ uv python install 3.12 --reinstall
Searching for Python versions matching: Python 3.12
Found existing installation for Python 3.12: cpython-3.12.5-macos-aarch64-none
Installed Python 3.12.6 in 2.44s
 - cpython-3.12.5-macos-aarch64-none
 + cpython-3.12.6-macos-aarch64-none
$ uv tool list
Python interpreter not found at `/Users/clm/Library/Application Support/uv/tools/pre-commit/bin/python3`
```

I can fix my tools easily enough by manually re-installing 3.12.5 (in co-existence with 3.12.6), but I can see two approaches to improve the user experience here:
- avoid uninstalling 3.12.5 (maybe raise a warning), seeing that it's used by tools (or at least ask for confirmation)
- bring all tools installed against 3.12.5 along the ride (this may not be doable in the general case but I confess it was what I *expected* to happen, but not necessarily silently)


---

_Comment by @zanieb on 2024-09-11 11:41_

Thanks for the report!

Related:

- https://github.com/astral-sh/uv/issues/6297
- https://github.com/astral-sh/uv/issues/4718

---

_Label `uv python` added by @zanieb on 2024-09-11 11:41_

---

_Label `uv tool` added by @zanieb on 2024-09-11 11:41_

---

_Label `bug` added by @zanieb on 2024-09-11 11:41_

---

_Comment by @neutrinoceros on 2024-09-11 12:06_

Thanks for the pointers. I looked for something similar before I opened this ticket but didn't find #6297. Feel free to close mine as a duplicate if it makes sense !

---

_Comment by @zanieb on 2024-09-11 13:15_

I'll tweak the title — we can track this separately, I think, even though it's similar.

---

_Renamed from "UX/BUG (?): support upgrading managed python for installed tools ?" to "Upgrade tool Python patch versions automatically on managed Python upgrade" by @zanieb on 2024-09-11 13:16_

---

_Closed by @jtfmumm on 2025-06-20 14:17_

---
