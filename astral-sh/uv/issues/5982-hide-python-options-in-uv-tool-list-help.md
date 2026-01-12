```yaml
number: 5982
title: "Hide Python options in `uv tool list` help"
type: issue
state: closed
author: zanieb
labels:
  - cli
assignees: []
created_at: 2024-08-09T22:51:19Z
updated_at: 2024-08-13T16:21:45Z
url: https://github.com/astral-sh/uv/issues/5982
synced_at: 2026-01-12T15:59:00Z
```

# Hide Python options in `uv tool list` help

---

_@zanieb_

We don't need `--python-preference` and `--no-python-downloads` here.

e.g.

```
❯ uv tool list --help
List installed tools

Usage: uv tool list [OPTIONS]

Options:
      --show-paths  Whether to display the path to each tool environment and installed executable

Cache options:
  -n, --no-cache               Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the
                               operation [env: UV_NO_CACHE=]
      --cache-dir <CACHE_DIR>  Path to the cache directory [env: UV_CACHE_DIR=]

Python options:
      --python-preference <PYTHON_PREFERENCE>  Whether to prefer uv-managed or system Python installations [possible values:
                                               only-managed, managed, system, only-system]
      --python-fetch <PYTHON_FETCH>            Whether to automatically download Python when required [possible values: automatic,
                                               manual]

...
```

---

_Label `cli` added by @zanieb on 2024-08-09 22:51_

---

_Closed by @zanieb on 2024-08-13 16:21_

---
