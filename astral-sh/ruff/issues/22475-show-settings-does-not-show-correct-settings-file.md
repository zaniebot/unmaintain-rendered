```yaml
number: 22475
title: Show-settings does not show correct settings file
type: issue
state: closed
author: olbermann
labels: []
assignees: []
created_at: 2026-01-09T10:59:18Z
updated_at: 2026-01-09T15:28:59Z
url: https://github.com/astral-sh/ruff/issues/22475
synced_at: 2026-01-10T11:10:00Z
```

# Show-settings does not show correct settings file

---

_Issue opened by @olbermann on 2026-01-09 10:59_

### Summary

I have a main directory `maindir` with a `pyproject.toml` containing one ruff configuration, and a subdirectory `subdir` with another `pyproject.toml` containing ruff configuration as well as an `example.py` file.
When I run, from the main directory, the command: 
`ruff check subdir/example.py --show-settings`, 
in line 2 of the output, it says 
`Settings path: maindir/pyproject.toml`
But (e.g also when removing the --show-settings option) it uses the ruff config from `maindir/subdir/pyproject.toml`, so I would have expected it to show me this as
`Settings path: maindir/subdir/pyproject.toml`

### Version

ruff 0.14.11

---

_Closed by @MichaReiser on 2026-01-09 15:28_

---
