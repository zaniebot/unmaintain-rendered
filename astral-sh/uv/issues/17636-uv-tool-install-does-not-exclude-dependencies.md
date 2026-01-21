```yaml
number: 17636
title: "`uv tool install` does not exclude dependencies defined in `exclude_dependencies`"
type: issue
state: open
author: FredrikBakken
labels:
  - bug
  - needs-decision
assignees: []
created_at: 2026-01-21T09:58:52Z
updated_at: 2026-01-21T14:31:20Z
url: https://github.com/astral-sh/uv/issues/17636
synced_at: 2026-01-21T15:05:52Z
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

_Comment by @zanieb on 2026-01-21 14:19_

`exclude-dependencies` isn't standard metadata and is only respected during operations within that project, not operations installing that project as a package.

We could consider reading it for local paths.

---

_Label `needs-decision` added by @zanieb on 2026-01-21 14:19_

---

_Comment by @FredrikBakken on 2026-01-21 14:31_

Awesome, @zanieb !

To provide some more context to the situation - it is a CLI tool that we use to communicate with Databricks using `databricks-connect`, however, since we also have another dependency that depends on `pyspark`, we're encountering a race condition between these two dependencies so that we have what feels like a 50/50 chance of getting a `AnalyzeArgument`-error. This is a known issue and also described here: https://docs.databricks.com/aws/en/dev-tools/databricks-connect/python/troubleshooting#conflicting-pyspark-installations

The solution for us has been to add `pyspark` to the `exclude_dependencies` definitions to avoid this conflict.

---
