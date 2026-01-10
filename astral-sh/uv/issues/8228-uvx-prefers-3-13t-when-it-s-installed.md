```yaml
number: 8228
title: "uvx prefers 3.13t when it's installed"
type: issue
state: closed
author: bluss
labels: []
assignees: []
created_at: 2024-10-15T20:50:56Z
updated_at: 2024-10-16T19:44:33Z
url: https://github.com/astral-sh/uv/issues/8228
synced_at: 2026-01-10T04:45:10Z
```

# uvx prefers 3.13t when it's installed

---

_Issue opened by @bluss on 2024-10-15 20:50_

I see that `uvx` prefers 3.13t when it's installed (even if 3.13.0 is also installed).

Example:

`uvx -v --from build python -c""`
```
DEBUG uv 0.4.22
DEBUG Searching for default Python interpreter in managed installations
DEBUG Searching for managed installations at `.local/share/uv/python`
DEBUG Found managed installation `cpython-3.13.0+freethreaded-linux-x86_64-gnu`
DEBUG Found `cpython-3.13.0+freethreaded-linux-x86_64-gnu` at `/home/user/.local/share/uv/python/cpython-3.13.0+freethreaded-linux-x86_64-gnu/bin/python3` (managed installations)
```

It is noticeable because it needs to build wheels from source much more often, and it fails to build some packages like jupyterlab, so 3.13t is not ready to be the default (not really news..).


uv 0.4.22

`uv python list`
```
cpython-3.13.0-linux-x86_64-gnu                 .local/share/uv/python/cpython-3.13.0-linux-x86_64-gnu/bin/python3 -> python3.13
cpython-3.13.0+freethreaded-linux-x86_64-gnu    .local/share/uv/python/cpython-3.13.0+freethreaded-linux-x86_64-gnu/bin/python3 -> python3.13
cpython-3.12.7-linux-x86_64-gnu                 .local/share/uv/python/cpython-3.12.7-linux-x86_64-gnu/bin/python3 -> python3.12
cpython-3.11.10-linux-x86_64-gnu                <download available>
cpython-3.11.9-linux-x86_64-gnu                 .local/share/uv/python/cpython-3.11.9-linux-x86_64-gnu/bin/python3 -> python3.11
cpython-3.10.15-linux-x86_64-gnu                <download available>
cpython-3.10.14-linux-x86_64-gnu                .local/share/uv/python/cpython-3.10.14-linux-x86_64-gnu/bin/python3 -> python3.10
cpython-3.9.20-linux-x86_64-gnu                 .local/share/uv/python/cpython-3.9.20-linux-x86_64-gnu/bin/python3 -> python3.9
cpython-3.9.19-linux-x86_64-gnu                 .local/share/uv/python/cpython-3.9.19-linux-x86_64-gnu/bin/python3 -> python3.9
cpython-3.8.20-linux-x86_64-gnu                 <download available>
cpython-3.8.19-linux-x86_64-gnu                 .local/share/uv/python/cpython-3.8.19-linux-x86_64-gnu/bin/python3 -> python3.8
cpython-3.7.9-linux-x86_64-gnu                  .local/share/uv/python/cpython-3.7.9-linux-x86_64-gnu/bin/python3 -> python3.7
``

---

_Comment by @zanieb on 2024-10-15 21:11_

Annoying, I thought I fixed this in #8191 — thanks for the report!

---

_Assigned to @zanieb by @zanieb on 2024-10-15 21:12_

---

_Closed by @zanieb on 2024-10-16 19:44_

---

_Closed by @zanieb on 2024-10-16 19:44_

---
