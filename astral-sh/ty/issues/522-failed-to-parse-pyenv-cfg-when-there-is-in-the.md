```yaml
number: 522
title: "Failed to parse pyenv.cfg when there is \"=\" in the command"
type: issue
state: closed
author: kkpattern
labels:
  - question
assignees: []
created_at: 2025-05-27T10:23:27Z
updated_at: 2025-05-27T10:32:15Z
url: https://github.com/astral-sh/ty/issues/522
synced_at: 2026-01-12T15:54:23Z
```

# Failed to parse pyenv.cfg when there is "=" in the command

---

_@kkpattern_

### Summary

Reproduce step:

```bash
python3 -m venv .venv --prompt hello_ty
. ./.venv/bin/activate
pip install ty
ty check .
```

output:

```bash
ty failed
  Cause: Invalid search path settings
  Cause: Failed to discover the site-packages directory: Failed to parse the pyvenv.cfg file at /home/.../ty-playground/.venv/pyvenv.cfg because line 6 has too many '=' characters
```

The pyvenv.cfg looks like this:

```
home = /usr/local/bin
include-system-site-packages = false
version = 3.11.12
prompt = 'hello_ty'
executable = /usr/local/bin/python3.11
command = /usr/local/bin/python3 -m venv --prompt="hello_ty" /home/.../ty-playground/.venv
```

I'm not sure if this is a bug with ty or python venv. Maybe the command should be escaped?

### Version

ty 0.0.0-alpha.7

---

_Comment by @MichaReiser on 2025-05-27 10:25_

Can you try a more recent ty version? This should have been fixed a few releases back (https://github.com/astral-sh/ruff/pull/18144)

---

_Label `question` added by @MichaReiser on 2025-05-27 10:25_

---

_Comment by @kkpattern on 2025-05-27 10:29_

Ah sorry. It's fine with `ty 0.0.1a7`.  I thought ty 0.0.0-alpha.7 is the latest version.

---

_Closed by @kkpattern on 2025-05-27 10:32_

---
