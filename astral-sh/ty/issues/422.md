```yaml
number: 422
title: "`error[unresolved-attribute]: Type '<module 'os'>' has no attribute 'sys'` - but `os.sys.winver` is there"
type: issue
state: closed
author: gr7ffo
labels:
  - question
  - imports
assignees: []
created_at: 2025-05-16T11:12:46Z
updated_at: 2025-05-16T12:08:14Z
url: https://github.com/astral-sh/ty/issues/422
synced_at: 2026-01-10T02:34:09Z
```

# `error[unresolved-attribute]: Type '<module 'os'>' has no attribute 'sys'` - but `os.sys.winver` is there

---

_Issue opened by @gr7ffo on 2025-05-16 11:12_

### Summary

`ty check` leaves me with
```
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[unresolved-attribute]: Type `<module 'os'>` has no attribute `sys`
  --> dtis-sgs2txt-helper.py:17:4
   |
16 | # Set local dev, Python 3.8 only runs on UT PC
17 | if os.sys.winver == '3.8':
   |    ^^^^^^
18 |     local = False
19 |     use_sgs2txt = True
   |
info: rule `unresolved-attribute` is enabled by default

Found 1 diagnostic
```
but `os.sys.winver` exists and is there, why than this `error[unresolved-attribute]`?

### Version

ty 0.0.1-alpha.3 (144a26d44 2025-05-15)

---

_Renamed from "error[unresolved-attribute]: Type `<module 'os'>` has no attribute `sys` - but os.sys.winver is there" to "`error[unresolved-attribute]: Type `<module 'os'>` has no attribute `sys`` - but `os.sys.winver` is there" by @gr7ffo on 2025-05-16 11:13_

---

_Renamed from "`error[unresolved-attribute]: Type `<module 'os'>` has no attribute `sys`` - but `os.sys.winver` is there" to "`error[unresolved-attribute]: Type '<module 'os'>' has no attribute 'sys'` - but `os.sys.winver` is there" by @gr7ffo on 2025-05-16 11:13_

---

_Comment by @danielhollas on 2025-05-16 11:16_

How exactly do you import os.sys?

---

_Comment by @sharkdp on 2025-05-16 11:44_

I think `os.sys` can not (and should not?) be accessed. It's [not re-exported](https://github.com/python/typeshed/blob/5785616f7a68f6923fd6032bcf9116e066a819b9/stdlib/os/__init__.pyi#L47) from `os` in typeshed. I understand that this is possible at runtime, but it's probably just a side-effect of the `sys`-import [here](https://github.com/python/cpython/blob/1566c34dc76ec6139e6827fbab6d76e084a63d9d/Lib/os.py#L26).

Is there an advantage to accessing it via `os`, or could you import `sys` directly?

Note that you might also need to [set the target platform](https://github.com/astral-sh/ty/blob/main/docs/reference/configuration.md#python-platform) to `win32`, if you're not on Windows in the first place.

See `environment` settings here: https://play.ty.dev/a2fda34d-6277-467b-ab68-173499840d6a

---

_Label `question` added by @sharkdp on 2025-05-16 11:45_

---

_Label `imports` added by @sharkdp on 2025-05-16 11:45_

---

_Comment by @carljm on 2025-05-16 12:04_

Mypy and pyright both also error on using `os.sys`. Mypy does provide a nice modified error message 'Module "os" does not explicitly export attribute "sys"', which we could consider adopting instead of just saying it doesn't exist.

Closing this issue since this is working as expected. If someone wants to open an issue for a distinct diagnostic message for this "exists but not exported" case, that would be fine. The awkward thing about that, as I see it, is that some typeshed non-re-exports really don't exist at runtime (they are just a typeshed implementation detail), while others (like `os.sys`) do really exist at runtime (they just aren't intended as part of the module's interface, and could disappear without warning in a future version of the module), and we have no way to tell the difference.

---

_Closed by @carljm on 2025-05-16 12:04_

---
