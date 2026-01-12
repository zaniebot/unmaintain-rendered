```yaml
number: 6154
title: "[Autofix error] UP032 Use f-string instead of `format` call"
type: issue
state: closed
author: jbradaric
labels:
  - bug
assignees: []
created_at: 2023-07-28T14:31:53Z
updated_at: 2023-07-28T17:52:44Z
url: https://github.com/astral-sh/ruff/issues/6154
synced_at: 2026-01-12T15:54:45Z
```

# [Autofix error] UP032 Use f-string instead of `format` call

---

_@jbradaric_

* A minimal code snippet that reproduces the bug (saved in /tmp/a.py)

```
'{} {}'.format(state, line if color is None else
               text_as_markup(line, color=color))
```

* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.

`ruff --target-version py311 --select UP032 --fix --isolated /tmp/a.py`

* The current Ruff version (`ruff --version`).
```
$ ruff --version
ruff 0.0.280
```

The following error occurs:
```
error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `/tmp/a.py`, the rule codes UP032, along with the `pyproject.toml` settings and executed com
mand, we'd be very appreciative!
```

The error doesn't occur if all the code is in a single line, without a line break after `else`.

---

_Label `bug` added by @charliermarsh on 2023-07-28 17:52_

---

_Comment by @charliermarsh on 2023-07-28 17:52_

Sorry about that, I just confirmed that this is fixed on `main` and should go out in Monday's release.

---

_Closed by @charliermarsh on 2023-07-28 17:52_

---
