---
number: 9936
title: "When installing app project, its `importlib.metadata` is not available"
type: issue
state: closed
author: rafalkrupinski
labels: []
assignees: []
created_at: 2024-12-16T14:05:24Z
updated_at: 2024-12-16T14:49:28Z
url: https://github.com/astral-sh/uv/issues/9936
synced_at: 2026-01-07T13:12:18-06:00
---

# When installing app project, its `importlib.metadata` is not available

---

_Issue opened by @rafalkrupinski on 2024-12-16 14:05_

I use a simple pattern to display version in my app projects, either in webapps or programs with CLI:
```py
def show_version()
  from importlib.metadata import version
  print(version("myproject"))
```

this works with projects managed by poetry, but not with `uv sync`

```shell
File "~/.local/share/uv/python/cpython-3.12.7-linux-x86_64-gnu/lib/python3.12/importlib/metadata/__init__.py", line 889, in version
    return distribution(distribution_name).version
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "~/.local/share/uv/python/cpython-3.12.7-linux-x86_64-gnu/lib/python3.12/importlib/metadata/__init__.py", line 862, in distribution
    return Distribution.from_name(distribution_name)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "~/.local/share/uv/python/cpython-3.12.7-linux-x86_64-gnu/lib/python3.12/importlib/metadata/__init__.py", line 399, in from_name
    raise PackageNotFoundError(name)
```

is there a way to force uv to install that metadata?

---

_Comment by @rafalkrupinski on 2024-12-16 14:49_

https://docs.astral.sh/uv/concepts/projects/config/#project-packaging

---

_Closed by @rafalkrupinski on 2024-12-16 14:49_

---
