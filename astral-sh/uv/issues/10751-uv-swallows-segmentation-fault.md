```yaml
number: 10751
title: uv swallows segmentation fault
type: issue
state: closed
author: tlinhart
labels:
  - bug
assignees: []
created_at: 2025-01-19T09:38:30Z
updated_at: 2025-01-24T15:20:33Z
url: https://github.com/astral-sh/uv/issues/10751
synced_at: 2026-01-10T04:27:58Z
```

# uv swallows segmentation fault

---

_Issue opened by @tlinhart on 2025-01-19 09:38_

While preparing another bug report, I accidentally found out that uv swallows a segmentation fault. Consider this minimal `test.py` script that causes a segfault:

```python
import os
os.kill(os.getpid(), 11)
```

Running it the usual way:

```shell
$ python3 test.py 
Segmentation fault (core dumped)
$ echo $?
139
```

Running it with uv:

```shell
$ uv run --script test.py
$ echo $?
1
```

Tested with v0.5.10 and v0.5.21 on x86 and v0.5.21 on ARM64, it that helps.

---

_Label `bug` added by @konstin on 2025-01-20 15:09_

---

_Closed by @konstin on 2025-01-24 15:20_

---
