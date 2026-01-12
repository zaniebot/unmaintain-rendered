```yaml
number: 12937
title: Prevent uninstalling a Python installation if it is used by a tool
type: pull_request
state: open
author: j178
labels: []
assignees: []
base: main
head: python-uninstall
created_at: 2025-04-17T10:03:10Z
updated_at: 2025-04-17T17:01:40Z
url: https://github.com/astral-sh/uv/pull/12937
synced_at: 2026-01-12T16:10:27Z
```

# Prevent uninstalling a Python installation if it is used by a tool

---

_@j178_

## Summary

This PR adds a check if a managed Python is still being used by any tool before uninstalling it. If it is, we won’t uninstall it unless you use the `--force` flag. This way, we avoid messing up any tools you have installed.

Could this be a breaking change?


---

_Assigned to @zanieb by @zanieb on 2025-04-17 17:01_

---
