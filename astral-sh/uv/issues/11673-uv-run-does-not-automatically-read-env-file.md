```yaml
number: 11673
title: uv run does not automatically read .env file
type: issue
state: closed
author: itviewer
labels:
  - bug
assignees: []
created_at: 2025-02-20T17:30:32Z
updated_at: 2025-02-20T17:47:37Z
url: https://github.com/astral-sh/uv/issues/11673
synced_at: 2026-01-12T16:00:43Z
```

# uv run does not automatically read .env file

---

_@itviewer_

### Summary

```
uv init demo
echo "MY_VAR='Hello, world'" > .env
```
main.py
```
import os

def main():
    print(os.getenv("MY_VAR"))


if __name__ == "__main__":
    main()
```
```
uv run main.py
None

uv run --env-file .env main.py
Hello, world
```

### Platform

Ubuntu 24.04 x86_64

### Version

uv 0.6.2

### Python version

Python 3.12.3

---

_Label `bug` added by @itviewer on 2025-02-20 17:30_

---

_Comment by @zanieb on 2025-02-20 17:47_

Please see https://github.com/astral-sh/uv/issues/9381 and https://github.com/astral-sh/uv/issues/1384#issuecomment-2455270920

---

_Closed by @zanieb on 2025-02-20 17:47_

---
