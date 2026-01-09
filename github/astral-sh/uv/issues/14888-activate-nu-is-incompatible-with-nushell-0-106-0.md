---
number: 14888
title: activate.nu is incompatible with nushell 0.106.0
type: issue
state: closed
author: AngelEzquerra
labels:
  - bug
  - help wanted
assignees: []
created_at: 2025-07-25T10:35:53Z
updated_at: 2025-09-05T21:13:01Z
url: https://github.com/astral-sh/uv/issues/14888
synced_at: 2026-01-07T13:12:19-06:00
---

# activate.nu is incompatible with nushell 0.106.0

---

_Issue opened by @AngelEzquerra on 2025-07-25 10:35_

### Summary

nushell 0.106.0 introduces a backwards incompatibility that affects activate.nu which now fails with the following error:

```
Warning: nu::parser::deprecated

  ⚠ Flag deprecated.
    ╭─[/Users/myuser/example/.venv/bin/activate.nu:49:27]
 48 │         } else {
 49 │           not ($env | get -i $name | is-empty)
    ·                           ─┬
    ·                            ╰── get --ignore-errors was deprecated in 0.106.0 and will be removed in a future release.
 50 │         }
    ╰────
  help: This flag has been renamed to `--optional (-o)` to better reflect its
        behavior.
```

I believe it could be fixed by changing `get -i $name` to `get --optional $name`.


### Platform

macOS 15.5

### Version

uv 0.8.3

### Python version

Python 3.13.5

---

_Label `bug` added by @AngelEzquerra on 2025-07-25 10:35_

---

_Comment by @Efterklang on 2025-07-25 13:36_

I solved the problem by manually modifying this line

https://github.com/astral-sh/uv/blob/1146f3f62d88bcfe6d81a45987b1fd4745b3eeb4/crates/uv-virtualenv/src/activator/activate.nu#L49


---

_Label `help wanted` added by @zanieb on 2025-07-25 22:49_

---

_Comment by @zanieb on 2025-07-25 22:49_

Thanks for the report!

---

_Referenced in [astral-sh/uv#14914](../../astral-sh/uv/pulls/14914.md) on 2025-07-26 06:12_

---

_Referenced in [astral-sh/uv#14917](../../astral-sh/uv/issues/14917.md) on 2025-07-26 19:33_

---

_Comment by @2bndy5 on 2025-08-14 02:54_

bump.

I was just about to file this as well.

---

_Referenced in [astral-sh/uv#15272](../../astral-sh/uv/pulls/15272.md) on 2025-08-14 08:54_

---

_Closed by @zanieb on 2025-09-05 21:13_

---
