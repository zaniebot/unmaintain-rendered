```yaml
number: 3304
title: Use fs_err for cachedir errors
type: pull_request
state: merged
author: konstin
labels:
  - bug
  - error messages
  - windows
assignees: []
merged: true
base: main
head: konsti/better-cache-dir-errors
created_at: 2024-04-29T10:32:31Z
updated_at: 2024-04-29T14:33:11Z
url: https://github.com/astral-sh/uv/pull/3304
synced_at: 2026-01-12T16:05:34Z
```

# Use fs_err for cachedir errors

---

_@konstin_

When running

```
set UV_CACHE_DIR=%LOCALAPPDATA%\uv\cache-foo && uv venv venv
```

in windows CMD, the error would be just

```
error: The system cannot find the path specified. (os error 3)
```

The problem is that the first action in the cache dir is adding the tag, and the `cachedir` crate is using `std::fs` instead of `fs_err`. I've copied the two functions we use from the crate and changed the import from `std::fs` to `fs_err`.

The new error is

```
error: failed to open file `C:\Users\Konstantin\AppData\Local\uv\cache-foo \CACHEDIR.TAG`
  Caused by: The system cannot find the path specified. (os error 3)
```

which correctly explains the problem.

Closes #3280

---

_Label `bug` added by @konstin on 2024-04-29 10:32_

---

_Label `windows` added by @konstin on 2024-04-29 10:32_

---

_@zanieb approved on 2024-04-29 14:28_

---

_Label `error messages` added by @zanieb on 2024-04-29 14:28_

---

_Merged by @konstin on 2024-04-29 14:33_

---

_Closed by @konstin on 2024-04-29 14:33_

---

_Branch deleted on 2024-04-29 14:33_

---
