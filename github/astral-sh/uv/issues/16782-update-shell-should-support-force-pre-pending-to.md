---
number: 16782
title: "`update-shell` should support force pre-pending to the `PATH`"
type: issue
state: open
author: zanieb
labels:
  - enhancement
assignees: []
created_at: 2025-11-19T20:45:53Z
updated_at: 2025-11-19T20:45:53Z
url: https://github.com/astral-sh/uv/issues/16782
synced_at: 2026-01-07T13:12:19-06:00
---

# `update-shell` should support force pre-pending to the `PATH`

---

_Issue opened by @zanieb on 2025-11-19 20:45_

### Summary

Currently, we will exit if the directory is on the `PATH` at all

https://github.com/astral-sh/uv/blob/d5257202662773b6794b1b2de6c490dd1404d7b5/crates/uv/src/commands/python/update_shell.rs#L45-L52

but this does not allow updating the SHELL in the case where the directory is at the end of the path and binaries are shadowed.

Either we should detect that the relevant binary is shadowed and do this automatically, or we should allow some sort of `--force` option to prepend even if it's present already.

### Example

```
uv python update-shell --force
```

---

_Label `enhancement` added by @zanieb on 2025-11-19 20:45_

---
