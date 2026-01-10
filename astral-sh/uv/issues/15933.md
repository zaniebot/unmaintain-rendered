```yaml
number: 15933
title: Interrupted self update will result in incomplete binary file
type: issue
state: open
author: criyle
labels:
  - bug
assignees: []
created_at: 2025-09-18T14:05:37Z
updated_at: 2026-01-09T14:56:45Z
url: https://github.com/astral-sh/uv/issues/15933
synced_at: 2026-01-10T03:11:35Z
```

# Interrupted self update will result in incomplete binary file

---

_Issue opened by @criyle on 2025-09-18 14:05_

### Summary

I was doing `uv self update` after I logged to a remote host in vs code, but it was interrupted by vs code because it wants to activate virtual environment for that terminal. Then the uv binary becomes incomplete and running the command will result in `segment fault`. I suspect the update is not atomic operation. 

### Platform

Red Hat Enterprise Linux 9.4 (Plow)

### Version

uv 0.8.16

### Python version

Python 3.12.11

---

_Label `bug` added by @criyle on 2025-09-18 14:05_

---

_Comment by @zanieb on 2025-09-18 14:12_

cc @Gankra 

---

_Comment by @EliteTK on 2026-01-09 14:56_

At least on linux, when the installer script does the [mv into the final location](https://github.com/axodotdev/cargo-dist/blob/5ae4cffc4ecf4f4d26b4d4e6970cad0f63cb0931/cargo-dist/templates/installer/installer.sh.j2#L641C16-L641C18), it does so from a tempdir, which can be on a different filesystem, in those cases we'll end up copying it rather than renaming it. So I think the fix there might just be to "move" to a temporary file in the same directory first before moving that file over the target binary/library. But we use cargo-dist for this?

Do we want to keep the fix in-tree, or should we report it upstream?

I could also be wrong and the issue could be somewhere else, but so far I couldn't find anything else which stuck out.

---
