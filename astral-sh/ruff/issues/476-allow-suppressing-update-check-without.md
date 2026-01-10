```yaml
number: 476
title: Allow suppressing update check without suppressing error messages
type: issue
state: closed
author: andersk
labels: []
assignees: []
created_at: 2022-10-26T16:52:53Z
updated_at: 2022-10-26T20:03:11Z
url: https://github.com/astral-sh/ruff/issues/476
synced_at: 2026-01-10T15:56:05Z
```

# Allow suppressing update check without suppressing error messages

---

_Issue opened by @andersk on 2022-10-26 16:52_

Ruff has frequent releases, which is great, but it also means that the “new version available” message frequently adds noise to the output. Another source of noise is the “No pyproject.toml found. Falling back to default configuration…” message. A linter that prints expected noise can drown out unexpected errors, especially when it’s run from a script alongside many other linters.

```
No pyproject.toml found.
Falling back to default configuration...
Found 1 error(s).
test.py:1:5: W292 No newline at end of file

A new version of ruff is available: v0.0.82 -> v0.0.83
Run to update: pip3 install --upgrade ruff
```

Currently the only ways to suppress the update check are

* at compile time, by disabling the `update-informer` feature—not an option for those installing binaries;
* at runtime, with the `-q`/`--quiet` option—not an option for those who want to see the actual diagnostics about the input code.

There’s no way to suppress the pyproject.toml message.

Can we have an option to print *nothing* unless there are diagnostics about the input code? Perhaps this should be what `-q` means, and the existing `-q` should be `-qq` or `-s`/`--silent`?

---

_Comment by @charliermarsh on 2022-10-26 17:58_

Agree with all of this. I think `-q` and `-s` make sense.

---

_Assigned to @charliermarsh by @charliermarsh on 2022-10-26 19:03_

---

_Closed by @charliermarsh on 2022-10-26 20:03_

---
