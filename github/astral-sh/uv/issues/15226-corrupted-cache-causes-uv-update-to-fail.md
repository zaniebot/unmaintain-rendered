---
number: 15226
title: corrupted cache? causes uv update to fail
type: issue
state: open
author: KRRT7
labels:
  - bug
assignees: []
created_at: 2025-08-11T20:27:48Z
updated_at: 2025-08-11T20:35:13Z
url: https://github.com/astral-sh/uv/issues/15226
synced_at: 2026-01-07T13:12:19-06:00
---

# corrupted cache? causes uv update to fail

---

_Issue opened by @KRRT7 on 2025-08-11 20:27_

### Summary

```
uv self update
info: Checking for updates...
error: The installation failed. Output from the installer: 
/var/folders/mg/k_c0twcj37q_gph3cfy3zlt80000gn/T/.tmp0RGmd7/installer.sh: line 1: !DOCTYPE: No such file or directory
: No such file or directory_gph3cfy3zlt80000gn/T/.tmp0RGmd7/installer.sh: line 2: !--
: command not found0twcj37q_gph3cfy3zlt80000gn/T/.tmp0RGmd7/installer.sh: line 3: 
/var/folders/mg/k_c0twcj37q_gph3cfy3zlt80000gn/T/.tmp0RGmd7/installer.sh: line 4: Hello: command not found
: command not found0twcj37q_gph3cfy3zlt80000gn/T/.tmp0RGmd7/installer.sh: line 6: 
/var/folders/mg/k_c0twcj37q_gph3cfy3zlt80000gn/T/.tmp0RGmd7/installer.sh: line 7: unexpected EOF while looking for matching `''
/var/folders/mg/k_c0twcj37q_gph3cfy3zlt80000gn/T/.tmp0RGmd7/installer.sh: line 83: syntax error: unexpected end of file
```

I'm suspecting it's the cache since `uv cache clean` fixed it but could be wrong

### Platform

macOS 

### Version

unk

### Python version

3.12

---

_Label `bug` added by @KRRT7 on 2025-08-11 20:27_

---

_Comment by @zanieb on 2025-08-11 20:35_

Hm shouldn't be related to the cache, the cargo-dist installer does not use the uv cache.

cc @Gankra 

---
