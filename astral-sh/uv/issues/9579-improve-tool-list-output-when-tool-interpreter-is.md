```yaml
number: 9579
title: "Improve `tool list` output when tool interpreter is missing"
type: issue
state: closed
author: zanieb
labels:
  - help wanted
  - error messages
  - cli
assignees: []
created_at: 2024-12-02T15:50:12Z
updated_at: 2025-01-09T21:48:08Z
url: https://github.com/astral-sh/uv/issues/9579
synced_at: 2026-01-12T15:59:53Z
```

# Improve `tool list` output when tool interpreter is missing

---

_@zanieb_

We currently show "Python interpreter not found at `...`" which is not very helpful, we should provide more context on what's going on.

```
❯ uv tool list
ansible-core v2.17.5
- ansible
- ansible-config
- ansible-connection
- ansible-console
- ansible-doc
- ansible-galaxy
- ansible-inventory
- ansible-playbook
- ansible-pull
- ansible-test
- ansible-vault
Python interpreter not found at `/Users/zb/.local/share/uv/tools/black/bin/python3`
```

```
❯ tree /Users/zb/.local/share/uv/tools/black/ -L 3
/Users/zb/.local/share/uv/tools/black/
├── CACHEDIR.TAG
├── bin
│   ├── activate
│   ├── activate.bat
│   ├── activate.csh
│   ├── activate.fish
│   ├── activate.nu
│   ├── activate.ps1
│   ├── activate_this.py
│   ├── black
│   ├── blackd
│   ├── deactivate.bat
│   ├── normalizer
│   ├── pydoc.bat
│   ├── python -> /Users/zb/.local/share/uv/python/cpython-3.13.0+freethreaded-macos-aarch64-none/bin/python3.13t
│   ├── python3 -> python
│   └── python3.13 -> python
├── lib
│   └── python3.13t
│       └── site-packages
├── pyvenv.cfg
└── uv-receipt.toml

5 directories, 18 files
```

---

_Label `cli` added by @zanieb on 2024-12-02 15:50_

---

_Label `error messages` added by @zanieb on 2024-12-02 15:56_

---

_Comment by @aretrace on 2025-01-04 20:54_

😅 what is going on?

---

_Comment by @zanieb on 2025-01-04 22:29_

The interpreter in `black`'s virtual environment is broken. So.. we should report this as a problem with that tool. This is easy to encounter if you upgrade or uninstall interpreters frequently, e.g., my setup is borked:

```
❯ uv tool list
Python interpreter not found at `/Users/zb/.local/share/uv/tools/ansible-core/bin/python3`
Python interpreter not found at `/Users/zb/.local/share/uv/tools/black/bin/python3`
fastapi v0.115.6
- fastapi
Python interpreter not found at `/Users/zb/.local/share/uv/tools/rooster-blue/bin/python3`
```

The warning if we're missing a receipt might be helpful in guiding how we should do this?

```
❯ rm /Users/zb/.local/share/uv/tools/rooster-blue/uv-receipt.toml
❯ uv tool list
Python interpreter not found at `/Users/zb/.local/share/uv/tools/ansible-core/bin/python3`
Python interpreter not found at `/Users/zb/.local/share/uv/tools/black/bin/python3`
fastapi v0.115.6
- fastapi
warning: Ignoring malformed tool `rooster-blue` (run `uv tool uninstall rooster-blue` to remove)
```

Perhaps we should hint how to reinstall since we have that information when a receipt is present?

---

_Label `help wanted` added by @zanieb on 2025-01-04 22:29_

---

_Comment by @aretrace on 2025-01-06 03:37_

I have encountered this issue and was unaware that it might arise from frequently upgrading or uninstalling interpreters (which I often do). Why does this happen?

---

_Comment by @zanieb on 2025-01-06 05:09_

If you remove the interpreter, the tool's virtual environment continues to reference it (on Unix, via a symbolic link), but it does not exist and therefore cannot be used.

We could detect that it's in use and prevent uninstallation, e.g., per https://github.com/astral-sh/uv/issues/4718, but there will always be cases we can't catch (i.e., virtual environments created outside of uv).

It's on our radar to improve the behavior here, just lots else to do :)

---

_Comment by @aretrace on 2025-01-07 04:23_

Ahh yes, tools need to run under some interpreter, I assume they just reference whatever interpreter that happens to be "selected" when the tool installs.

---

_Closed by @zanieb on 2025-01-09 21:48_

---
