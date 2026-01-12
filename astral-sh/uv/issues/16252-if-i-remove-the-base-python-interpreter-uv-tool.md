```yaml
number: 16252
title: "If I remove the base Python interpreter, `uv tool list` doesn't work"
type: issue
state: open
author: pfmoore
labels:
  - bug
  - windows
assignees: []
created_at: 2025-10-11T11:59:51Z
updated_at: 2025-12-18T14:38:30Z
url: https://github.com/astral-sh/uv/issues/16252
synced_at: 2026-01-12T16:02:27Z
```

# If I remove the base Python interpreter, `uv tool list` doesn't work

---

_@pfmoore_

### Summary

I uninstalled Python 3.13 from my system, and reinstalled it using the new Python Manager. As a result, the old `python.exe` no longer exists. Now, when I run `uv tool list` I get:

```
warning: Querying Python at `C:\Users\Gustav\AppData\Roaming\uv\tools\nox\Scripts\python.exe` failed with exit status exit code: 103

[stderr]
did not find executable at 'C:\Users\Gustav\AppData\Local\Programs\Python\Python313\python.exe': The system cannot find the path specified.
 (run `uv tool install nox --reinstall` to reinstall)
```

This is reasonable (although it would be nice if the error was handled better, maybe by listing `nox` but with a note that the Python executable wasn't found, rather than a raw error). But what *isn't* OK is that the recommended action, `uv tool install nox --reinstall`, fails with the same error. Reinstalling a broken tool should work, not fail with the same error that is the reason you're trying to reinstall!

Uninstalling nox (and then installing it from scratch) did work, although I'd prefer it if reeinstalling could be made to work, rather than changing the recommendation to uninstall and then reinstall.

As a side note, if I had been able to reinstall nox, it's not obvious to me whether I'd expect it to be updated to use Python 3.14, or to still try to use Python 3.13 (and fail because I don't have Python 3.13 installed) - although I'm fine if it worked like uninstall/reinstall and used the default system Python (3.14). I definitely *wouldn't* want it to install a uv-managed copy of Python, though - I'm careful to manage my persistent Python installations via the standard Python installer, not via uv, and I wouldn't want that to change.

### Platform

Windows 11 x86_64

### Version

uv 0.9.2 (141369ce7 2025-10-10)

### Python version

Python 3.14.0 (although nox was installed using Python 3.13)

---

_Label `bug` added by @pfmoore on 2025-10-11 11:59_

---

_Label `windows` added by @konstin on 2025-12-18 14:19_

---

_Comment by @konstin on 2025-12-18 14:38_

This bug is Windows-specific, we're handling this case correctly on Unix (with a warning and hint, non-fatal). Draft PR with a fix: https://github.com/astral-sh/uv/pull/17176

> I definitely _wouldn't_ want it to install a uv-managed copy of Python, though - I'm careful to manage my persistent Python installations via the standard Python installer, not via uv, and I wouldn't want that to change.

The best way to ensure that is to have a global `uv.toml` that sets the python preference to only-system, then uv won't use managed interpreters at all (https://docs.astral.sh/uv/reference/settings/#python-preference).

> As a side note, if I had been able to reinstall nox, it's not obvious to me whether I'd expect it to be updated to use Python 3.14, or to still try to use Python 3.13 (and fail because I don't have Python 3.13 installed) - although I'm fine if it worked like uninstall/reinstall and used the default system Python (3.14).

The current Unix behavior that the PR ports to Windows is that `--reinstall` with a broken existing venv looks for the system default, unless otherwise specified e.g. with `-p`, so it will use 3.14. That's specific for broken interpreters, which doesn't allow us to reuse the actual venv interpreter, so we fall back to standard priorities (we could try reading the version from `pyvenv.cfg`, but I'm not sure if the experience is better if we force 3.13 with `--reinstall` without the user having requested it with `-p`)

---
