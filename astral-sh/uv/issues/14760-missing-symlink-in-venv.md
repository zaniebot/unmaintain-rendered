```yaml
number: 14760
title: missing symlink in venv
type: issue
state: closed
author: dotysan
labels:
  - bug
assignees: []
created_at: 2025-07-20T19:07:25Z
updated_at: 2025-07-21T16:25:51Z
url: https://github.com/astral-sh/uv/issues/14760
synced_at: 2026-01-12T16:01:56Z
```

# missing symlink in venv

---

_@dotysan_

### Summary

UV adds only 3 symlinks to the path.

`uv --quiet venv --managed-python --python=3.13t`
`ls -oh .venv/bin/python*`
```
lrwxrwxrwx 1 curtis 96 Jul 20 18:50 .venv/bin/python -> /home/curtis/.local/share/uv/python/cpython-3.13.5+freethreaded-linux-x86_64-gnu/bin/python3.13t
lrwxrwxrwx 1 curtis  6 Jul 20 18:50 .venv/bin/python3 -> python
lrwxrwxrwx 1 curtis  6 Jul 20 18:50 .venv/bin/python3.13 -> python
```

However, the venv module adds 4.

`rm -fr .venv`
`python3.13t -m venv .venv`
`ls -oh .venv/bin/python*`
```
lrwxrwxrwx 1 curtis 11 Jul 20 18:51 .venv/bin/python -> python3.13t
lrwxrwxrwx 1 curtis 11 Jul 20 18:51 .venv/bin/python3 -> python3.13t
lrwxrwxrwx 1 curtis 11 Jul 20 18:51 .venv/bin/python3.13 -> python3.13t
lrwxrwxrwx 1 curtis 20 Jul 20 18:51 .venv/bin/python3.13t -> /usr/bin/python3.13t
```

So with the system /usr/bin/python3.13t in my path but missing in the venv, this would fail due to the system python being found before the venv.

`uv pip install orjson`

```py
#! /usr/bin/env python3.13t

import orjson
```
```
ModuleNotFoundError: No module named 'orjson'
```

Could we have a .venv/bin/python3.13t symlink please?

I believe this pertains to 3.14 as well.

### Platform

Linux 6 x86_64

### Version

uv 0.8.0

### Python version

3.13.5t

---

_Label `bug` added by @dotysan on 2025-07-20 19:07_

---

_Comment by @dotysan on 2025-07-20 19:10_

Or more simply, without a system python3.13t `env python3.13t` fails completely even in a valid uv venv.
```
env: ‘python3.13t’: No such file or directory
```

---

_Assigned to @charliermarsh by @charliermarsh on 2025-07-20 21:32_

---

_Comment by @charliermarsh on 2025-07-20 21:32_

Will take a look, thanks.

---

_Closed by @charliermarsh on 2025-07-21 16:25_

---
