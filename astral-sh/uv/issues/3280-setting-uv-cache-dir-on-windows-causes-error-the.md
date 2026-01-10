```yaml
number: 3280
title: "Setting UV_CACHE_DIR on Windows causes \"error: The system cannot find the path specified. (os error 3)\""
type: issue
state: closed
author: gozdal
labels:
  - bug
  - windows
assignees: []
created_at: 2024-04-26T16:07:28Z
updated_at: 2024-04-29T14:33:11Z
url: https://github.com/astral-sh/uv/issues/3280
synced_at: 2026-01-10T05:31:37Z
```

# Setting UV_CACHE_DIR on Windows causes "error: The system cannot find the path specified. (os error 3)"

---

_Issue opened by @gozdal on 2024-04-26 16:07_

I am trying to work around another issue in [concurrently using the same cache directory](https://github.com/astral-sh/uv/issues/2810) by setting `UV_CACHE_DIR`.
If I do
```
set UV_CACHE_DIR=%LOCALAPPDATA%\uv\cache-foo && uv venv --help
```

I correctly see
```
      --cache-dir <CACHE_DIR>
          Path to the cache directory.

          Defaults to `$HOME/Library/Caches/uv` on macOS, `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on Linux, and
          `$HOME/.cache/<project_path> {FOLDERID_LocalAppData}/<project_path>/cache/uv` on Windows.

          [env: UV_CACHE_DIR=C:\Users\foo\AppData\Local\uv\cache-foo ]
```

However, if I want to create a venv I get:
`set UV_CACHE_DIR=%LOCALAPPDATA%\uv\cache-foo && uv venv venv`
```
error: The system cannot find the path specified. (os error 3)
```

`uv 0.1.33 (2cee7525c 2024-04-17)`, on Windows Server 2016 and 2022

---

_Label `windows` added by @konstin on 2024-04-29 09:52_

---

_Comment by @konstin on 2024-04-29 10:33_

You've hit an interesting edge case: The path contains a trailing space, where we can create the directory, but writing a file joined to that directory fails.

#3304 should make this clearer in the error message:

```
error: failed to open file `C:\Users\Konstantin\AppData\Local\uv\cache-foo \CACHEDIR.TAG`
  Caused by: The system cannot find the path specified. (os error 3)
```

---

_Assigned to @konstin by @konstin on 2024-04-29 10:40_

---

_Label `bug` added by @konstin on 2024-04-29 10:40_

---

_Comment by @gozdal on 2024-04-29 12:25_

That's interesting, wouldn't think that `set foo=bar && cmd` would set `foo=bar` with a space at the end?

---

_Closed by @konstin on 2024-04-29 14:33_

---
