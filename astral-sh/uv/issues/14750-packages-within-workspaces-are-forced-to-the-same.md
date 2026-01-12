```yaml
number: 14750
title: Packages within workspaces are forced to the same version constraints as unrelated packages
type: issue
state: open
author: winstxnhdw
labels:
  - bug
assignees: []
created_at: 2025-07-20T02:18:55Z
updated_at: 2025-07-20T02:32:38Z
url: https://github.com/astral-sh/uv/issues/14750
synced_at: 2026-01-12T16:01:56Z
```

# Packages within workspaces are forced to the same version constraints as unrelated packages

---

_@winstxnhdw_

### Summary

I have the following packages in my `uv` workspace with the following Python version constraints.

- server (>=3.13, <3.14)
- capgen-cli (>=3.9)
- transcription (>=3.9)
- captions (>=3.9)

Executing the following command on the `capgen-cli` package,

```bash
uv run --python 3.9 --package capgen-cli pyright
```

gives the following error.

```bash
Using CPython 3.9.20
error: The requested interpreter resolved to Python 3.9.20, which is incompatible with the project's Python requirement: `>=3.13.5, <3.14` (from workspace member `server`'s `project.requires-python`).
```

This is unexpected behaviour because `capgen-cli` does not depend on `server` nor `capgen`. However, building `capgen-cli` has no issues but I would love to be able to run my linting and type-checking on CI on without passing the `--isolated` flag.

### Platform

Arch Linux

### Version

uv 0.7.18 (87e9ccfb9 2025-07-01)

### Python version

N/A

---

_Label `bug` added by @winstxnhdw on 2025-07-20 02:18_

---

_Assigned to @zanieb by @zanieb on 2025-07-20 02:21_

---
