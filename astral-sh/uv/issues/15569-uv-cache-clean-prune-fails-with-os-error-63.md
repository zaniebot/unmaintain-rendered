---
number: 15569
title: "uv cache {clean, prune} fails with os error 63"
type: issue
state: open
author: barberogaston
labels:
  - bug
assignees: []
created_at: 2025-08-28T11:53:26Z
updated_at: 2025-11-03T19:25:49Z
url: https://github.com/astral-sh/uv/issues/15569
synced_at: 2026-01-10T01:25:57Z
---

# uv cache {clean, prune} fails with os error 63

---

_Issue opened by @barberogaston on 2025-08-28 11:53_

### Summary

Current state: `uv`'s cache folder is quite bloated with 57GiB of data:

```
❯ du -h -d 1 | sort -hr | head -n 10
 57G    ./uv
```

However, trying to run a `uv cache clean` or `uv cache prune` yields the following error:

```
❯ uv cache clean
Clearing cache at: uv
error: Failed to clear cache at: uv
  Caused by: IO error for operation on /Users/gaston.barbero/.cache/uv/sdists-v6/index/4306dec2056c9470/xgboost/1.3.3/epUeJwNL75qLZYa8tNqAC/src/build/temp.macosx-11.0-arm64-cpython-312/xgboost/src/build/temp.macosx-11.0-arm64-cpython-312/xgboost/src/build/temp.macosx-11.0-arm64-cpython-312/xgboost/src/build/temp.macosx-11.0-arm64-cpython-312/xgboost/src/build/temp.macosx-11.0-arm64-cpython-312/xgboost/src/build/temp.macosx-11.0-arm64-cpython-312/xgboost/src/build/temp.macosx-11.0-arm64-cpython-312/xgboost/src/build/temp.macosx-11.0-arm64-cpython-312/xgboost/src/build/temp.macosx-11.0-arm64-cpython-312/xgboost/src/build/temp.macosx-11.0-arm64-cpython-312/xgboost/src/build/temp.macosx-11.0-arm64-cpython-312/xgboost/src/build/temp.macosx-11.0-arm64-cpython-312/xgboost/src/build/temp.macosx-11.0-arm64-cpython-312/xgboost/src/build/temp.macosx-11.0-arm64-cpython-312/xgboost/src/build/temp.macosx-11.0-arm64-cpython-312/xgboost/src/build/temp.macosx-11.0-arm64-cpython-312/xgboost/src/xgboost/dmlc-core/tracker/yarn/src/main/java/org/apache/hadoop/yarn/dmlc: File name too long (os error 63)
  Caused by: File name too long (os error 63)
```

### Platform

macOS M1 Sonoma 14.7.7 (Darwin 23.6.0 arm64)

### Version

v0.8.13

### Python version

3.12.6

---

_Label `bug` added by @barberogaston on 2025-08-28 11:53_

---

_Comment by @charliermarsh on 2025-08-28 12:03_

You can just run `rm` on the output of `uv cache dir` as a workaround.

---

_Comment by @barberogaston on 2025-08-28 12:11_

> You can just run `rm` on the output of `uv cache dir` as a workaround.

Yup, that's the path I went for

---

_Comment by @charliermarsh on 2025-08-28 12:19_

(We should make the removal operations robust to this, though.)

---

_Comment by @sunfkny on 2025-11-02 13:04_

Cache clean also fails when there is a Windows-incompatible file name (e.g. ending with a period) in the cache dir

```
PS C:\Users\root> uv cache clean
Clearing cache at: AppData\Local\uv\cache
error: Failed to clear cache at: AppData\Local\uv\cache
  Caused by: failed to remove file `C:\Users\root\AppData\Local\uv\cache\sdists-v9\index\3ba65c4f41aac3a1\uwsgi\2.0.31\l1w3t3Nrz8OnzxF6ri3eg\src\core\logging.`: The system cannot find the file specified. (os error 2)
PS C:\Users\root> python
Python 3.14.0 (tags/v3.14.0:ebf955d, Oct  7 2025, 10:15:03) [MSC v.1944 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import os
>>> os.path.isreserved(r"C:\Users\root\AppData\Local\uv\cache\sdists-v9\index\3ba65c4f41aac3a1\uwsgi\2.0.31\l1w3t3Nrz8OnzxF6ri3eg\src\core\logging.")
True
>>>
```

workaround:
```shell
# cmd
rd /s /q "C:\Users\root\AppData\Local\uv\cache"
# powershell
rm -Recurse -Force "C:\Users\root\AppData\Local\uv\cache"
```

programmatically delete:
```rust
fs_err::remove_file(r"\\?\C:\Users\root\AppData\Local\uv\cache\sdists-v9\index\3ba65c4f41aac3a1\uwsgi\2.0.31\l1w3t3Nrz8OnzxF6ri3eg\src\core\logging.").unwrap();
// The \\?\ prefix allows operations on paths that exceed the MAX_PATH limit or contain characters normally restricted by the Win32 API (for example, trailing dots or spaces).
// See: https://learn.microsoft.com/en-us/windows/win32/fileio/naming-a-file#win32-file-namespaces
```

---

_Comment by @konstin on 2025-11-03 18:57_

@sunfkny That looks like a different problem, can you open a separate bug with complete instructions for reproducing this?

---

_Comment by @zanieb on 2025-11-03 19:25_

I'm actually not sure _how_ to make removal robust to this?

The best I can think of is to update https://github.com/astral-sh/uv/blob/00aa2ab67290f45c3797c1359d027cc38d137d7e/crates/uv-cache/src/removal.rs#L217 to emit a warning and ignore the error.

---

_Referenced in [astral-sh/uv#16586](../../astral-sh/uv/issues/16586.md) on 2025-11-04 07:45_

---
