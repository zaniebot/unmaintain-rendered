```yaml
number: 15667
title: "`uv python list` errors with `Exec format error` on non-native installation"
type: issue
state: open
author: zanieb
labels:
  - bug
assignees: []
created_at: 2025-09-03T17:56:24Z
updated_at: 2025-09-03T17:56:54Z
url: https://github.com/astral-sh/uv/issues/15667
synced_at: 2026-01-10T03:23:54Z
```

# `uv python list` errors with `Exec format error` on non-native installation

---

_Issue opened by @zanieb on 2025-09-03 17:56_

> With `uv` version `0.8.15` on Linux, I'm getting a related issue leading to `uv python list` failing. It works until there is some cross-platform installation in the directory. I'm doing this in order to build cross-platform libraries.
> 
> ```sh
> marcel@fred:~$ uv python list
> cpython-3.14.0rc2-linux-x86_64-gnu                 <download available>
> cpython-3.14.0rc2+freethreaded-linux-x86_64-gnu    <download available>
> cpython-3.13.7-linux-x86_64-gnu                    <download available>
> cpython-3.13.7+freethreaded-linux-x86_64-gnu       <download available>
> cpython-3.13.5-linux-x86_64-gnu                    .local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/bin/python3.13
> cpython-3.12.11-linux-x86_64-gnu                   .local/share/uv/python/cpython-3.12.11-linux-x86_64-gnu/bin/python3.12
> cpython-3.12.3-linux-x86_64-gnu                    /usr/bin/python3.12
> cpython-3.12.3-linux-x86_64-gnu                    /usr/bin/python3 -> python3.12
> cpython-3.11.13-linux-x86_64-gnu                   .local/share/uv/python/cpython-3.11.13-linux-x86_64-gnu/bin/python3.11
> cpython-3.10.18-linux-x86_64-gnu                   <download available>
> cpython-3.9.23-linux-x86_64-gnu                    <download available>
> cpython-3.8.20-linux-x86_64-gnu                    <download available>
> pypy-3.11.13-linux-x86_64-gnu                      <download available>
> pypy-3.10.16-linux-x86_64-gnu                      <download available>
> pypy-3.9.19-linux-x86_64-gnu                       <download available>
> pypy-3.8.16-linux-x86_64-gnu                       <download available>
> graalpy-3.11.0-linux-x86_64-gnu                    <download available>
> graalpy-3.10.0-linux-x86_64-gnu                    <download available>
> graalpy-3.8.5-linux-x86_64-gnu                     <download available>
> ```
> ```sh
> marcel@fred:~$ uv python install cpython-3.13.7-macos-aarch64-none
> Installed Python 3.13.7 in 316ms
>  + cpython-3.13.7-macos-aarch64-none (python3.13)
> ```
> ```sh
> marcel@fred:~$ uv python list
> error: Failed to inspect Python interpreter from first executable in the search path at `.local/bin/python3.13`
>   Caused by: Failed to query Python interpreter at `/home/marcel/.local/bin/python3.13`
>   Caused by: Exec format error (os error 8)
> ``` 

 _Originally posted by @marcelroed in [#11940](https://github.com/astral-sh/uv/issues/11940#issuecomment-3250057715)_

---

_Label `bug` added by @zanieb on 2025-09-03 17:56_

---

_Assigned to @zanieb by @zanieb on 2025-09-03 17:56_

---
