```yaml
number: 11258
title: "`ruff server` does not merge editor settings with fallback settings"
type: issue
state: closed
author: snowsignal
labels:
  - bug
  - server
assignees: []
created_at: 2024-05-03T01:52:24Z
updated_at: 2024-05-04T17:52:02Z
url: https://github.com/astral-sh/ruff/issues/11258
synced_at: 2026-01-12T15:54:50Z
```

# `ruff server` does not merge editor settings with fallback settings

---

_@snowsignal_

If no file-based configuration exists for a workspace, only the fallback settings are used, and they are not combined with editor settings. The expected behavior is that editor settings should still be merged with fallback settings.

---

_Label `bug` added by @snowsignal on 2024-05-03 01:52_

---

_Label `server` added by @snowsignal on 2024-05-03 01:52_

---

_Assigned to @snowsignal by @snowsignal on 2024-05-03 01:52_

---

_Comment by @akthe-at on 2024-05-04 01:56_

@snowsignal Does this PR solve this scenario:
Windows 10
Neovim Nightly Latest
Ruff 0.4.3
Python 3.12.3

- Have global config in ~/.config/ruff/pyproject.toml
- Create "test.py" in  ~/ (not a project/workspace and there is no present .toml config)
- Create a test function
```python
def this_function(y: int, x: int) -> int:


    """ This is an example function """


    word_fx: int = 0
    return y + x + word_fx
```

- Notice the excessive space between function definition and doc strings...
- Attempt to save (my neovim config would use conform.nvim to call ruff to format on save)
- 2-4 second hang...ruff errors and times out (   Warn  8:53:50 PM notify.warn [LSP][ruff] timeout)
- Curve ball...wait a minute or two to type this example/report up...go to save and it formats successfully this time.
- This does not occur in projects with a local .toml file...
- I am assuming this is because it takes time to look for the global config and it doesn't merge with the editor config without the local config? 

---

_Closed by @snowsignal on 2024-05-04 17:52_

---
