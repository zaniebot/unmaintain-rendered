```yaml
number: 9466
title: Escape path in batch script
type: pull_request
state: closed
author: konstin
labels:
  - bug
assignees: []
base: main
head: konsti/batch-escaping
created_at: 2024-11-27T12:35:10Z
updated_at: 2024-11-28T09:12:04Z
url: https://github.com/astral-sh/uv/pull/9466
synced_at: 2026-01-12T16:08:49Z
```

# Escape path in batch script

---

_@konstin_

## Summary

In the batch activator script, we need to escape double quotes with an additional double quote (https://stackoverflow.com/a/562080/3549270).

See #9424.

## Test Plan

Unclear.

---

_Label `bug` added by @konstin on 2024-11-27 12:35_

---

_Comment by @konstin on 2024-11-28 09:12_

I failed to test this since double quotes are not allowed in file names (https://learn.microsoft.com/en-us/windows/win32/fileio/naming-a-file#naming-conventions)

---

_Closed by @konstin on 2024-11-28 09:12_

---
