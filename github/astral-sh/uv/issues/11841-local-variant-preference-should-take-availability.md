---
number: 11841
title: Local variant preference should take availability of distributions into account
type: issue
state: open
author: paveldikov
labels:
  - bug
assignees: []
created_at: 2025-02-27T22:05:44Z
updated_at: 2025-02-28T01:27:19Z
url: https://github.com/astral-sh/uv/issues/11841
synced_at: 2026-01-07T13:12:18-06:00
---

# Local variant preference should take availability of distributions into account

---

_Issue opened by @paveldikov on 2025-02-27 22:05_

### Summary

#11546 introduces a bias for local versions. However, let's take this scenario into consideration:
```
foo-1.2.3
---
sdist
windows cp310 wheel
windows cp311 wheel
windows cp312 wheel
linux x86-64 cp310 wheel
... various other wheels ...


foo-1.2.3+special
---
linux x86-64 cp311 wheel
```
Notice how `1.2.3+special` only provides wheels for a few specific combinations. This is something we do quite a lot internally, when we produce in-house wheel builds of OSS libraries.

I generate a universal constraints file using `uv pip compile -p 3.10 --universal` and I get
```
foo==1.2.3 ; platform_machine != 'x86_64' or sys_platform != 'linux'
foo==1.2.3+special ; platform_machine == 'x86_64' and sys_platform == 'linux'
```

Attempting to install this on `linux x86_64 cp310` (or `cp312`) will fail because `+special` is not actually satisfiable due to lack of matching distribution. The non-special one, however, is.

Ideally we'd instead get
```
foo==1.2.3 ; platform_machine != 'x86_64' or sys_platform != 'linux' or python_full_version != "3.11.*" or python_implementation != 'CPython'
foo==1.2.3+special ; platform_machine == 'x86_64' and sys_platform == 'linux' and python_full_version == "3.11.*" and python_implementation == 'CPython'
```

### Platform

Linux

### Version

0.6.3

### Python version

3.12.3

---

_Label `bug` added by @paveldikov on 2025-02-27 22:05_

---

_Renamed from "Local variant preference should take Python version/flavour into account" to "Local variant preference should take availability of distributions into account" by @paveldikov on 2025-02-27 22:21_

---

_Comment by @charliermarsh on 2025-02-28 00:15_

We already do this, but it doesn't take Python version into account, just platform availability.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-02-28 00:19_

---

_Comment by @paveldikov on 2025-02-28 01:10_

Yeah, exactly, it's considering some but not all dimensions. Python flavour is also another one I imagine (my use case is CPython-only but I imagine others might care)

---

_Comment by @charliermarsh on 2025-02-28 01:27_

Agreed, make sense.

---

_Referenced in [astral-sh/uv#11294](../../astral-sh/uv/pulls/11294.md) on 2025-02-28 19:36_

---
