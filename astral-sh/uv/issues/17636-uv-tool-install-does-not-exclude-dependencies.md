```yaml
number: 17636
title: "`uv tool install` does not exclude dependencies defined in `exclude_dependencies`"
type: issue
state: open
author: FredrikBakken
labels:
  - bug
assignees: []
created_at: 2026-01-21T09:58:52Z
updated_at: 2026-01-21T09:58:52Z
url: https://github.com/astral-sh/uv/issues/17636
synced_at: 2026-01-21T11:00:05Z
```

# `uv tool install` does not exclude dependencies defined in `exclude_dependencies`

---

_@FredrikBakken_

### Summary

When installing a Python project as an `uv tool` using the local path, e.g.:

```shell
uv tool install <path>
```

I expect it to respect the `exclude-dependencies` list defined in the `pyproject.toml` file. However, this does not seem to be the case as I can find the excluded dependencies that were defined inside the `site-packages` path, e.g.: `/home/<user>/.local/share/uv/tools/<tool-name>/lib/python3.11/site-packages`

### Platform

Linux 6.6.87.2-microsoft-standard-WSL2 x86_64 GNU/Linux

### Version

uv 0.9.24

### Python version

Python 3.14.2

---

_Label `bug` added by @FredrikBakken on 2026-01-21 09:58_

---
