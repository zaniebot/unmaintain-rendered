```yaml
number: 12663
title: Uv run or tools lockfile
type: issue
state: open
author: gsemet
labels:
  - enhancement
assignees: []
created_at: 2025-04-04T06:40:32Z
updated_at: 2025-04-04T06:40:32Z
url: https://github.com/astral-sh/uv/issues/12663
synced_at: 2026-01-10T03:41:47Z
```

# Uv run or tools lockfile

---

_Issue opened by @gsemet on 2025-04-04 06:40_

### Summary

Hello. Sorry if this request has already done but i did not find my request in my search on the issue tracker.

What I want is:
- when I am in a folder A, I want ‘uv run’ to take tool T in version v1 (+ locked dependencies)
- when I am in folder (or project) B, T is in v2
i need a lockfile but not for the virtualenv, for the tools used in this env. this is a production use case (not a development use case). With poetry I would use a package-mode=false but this is not the most elegant nor efficient solution.

Potentially I need two tools T1 and T2 that have incompatible dependencies so that I cannot install in the same venv.

This is more or less what you can have with nix, but I find it over complicated just to select a special version of a tool.

### Example

_No response_

---

_Label `enhancement` added by @gsemet on 2025-04-04 06:40_

---
