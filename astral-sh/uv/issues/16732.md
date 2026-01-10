```yaml
number: 16732
title: "`--with-requirements script` doesn't work if script doesn't end with `.py`"
type: issue
state: closed
author: janlarres
labels:
  - bug
assignees: []
created_at: 2025-11-14T06:56:23Z
updated_at: 2025-11-21T02:33:43Z
url: https://github.com/astral-sh/uv/issues/16732
synced_at: 2026-01-10T03:23:55Z
```

# `--with-requirements script` doesn't work if script doesn't end with `.py`

---

_Issue opened by @janlarres on 2025-11-14 06:56_

### Summary

Save this as file `test`:

```python
#!/usr/bin/env -S uv run --script
# /// script
# dependencies = ["requests"]
# ///

import requests

requests.get("https://google.com")
```

Then try to run something like ty in its environment:

```
$ uvx --with-requirements test ty check test
error: Couldn't parse requirement in `test` at position 84
  Caused by: Expected one of `@`, `(`, `<`, `=`, `>`, `~`, `!`, `;`, found `r`
import requests
       ^
```

If you rename it to `test.py` it works fine:

```
$ uvx --with-requirements test.py ty check test.py
Installed 6 packages in 5ms
All checks passed!
```

Since inline metadata is meant for standalone scripts that usually wouldn't have an extension, it would be really useful if this worked.

### Platform

Linux 6.16.3+deb14-amd64 x86_64 GNU/Linux

### Version

uv 0.9.9

### Python version

Python 3.14.0

---

_Label `bug` added by @janlarres on 2025-11-14 06:56_

---

_Comment by @nooscraft on 2025-11-14 09:35_

The fix is to first check whether the file contains PEP 723 inline metadata (# /// script block) before treating it as a requirements.txt. That way, we can avoid misclassifying scripts that embed their own dependencies?
@konstin 

---

_Closed by @charliermarsh on 2025-11-21 02:33_

---
