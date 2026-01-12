```yaml
number: 1389
title: Improve error message for invalid sdist archives
type: pull_request
state: merged
author: robin-nitrokey
labels:
  - error messages
assignees: []
merged: true
base: main
head: invalid-archive-error
created_at: 2024-02-15T23:46:10Z
updated_at: 2024-02-16T00:03:56Z
url: https://github.com/astral-sh/uv/pull/1389
synced_at: 2026-01-12T16:04:36Z
```

# Improve error message for invalid sdist archives

---

_@robin-nitrokey_

This PR improves the error message for the problem described in https://github.com/astral-sh/uv/issues/1376.  The original output duplicates the actual error message and includes lots of noise (`DirEntry { inner: DirEntry(...) }`).

```
$ uv pip install hexdump==3.3
error: Failed to download and build: hexdump==3.3
  Caused by: Failed to extract source distribution: The top level of the archive must only contain a list directory, but it contains: [DirEntry { inner: DirEntry("/home/robin/.cache/uv/.tmpgSvTCk/__main__.py") }, DirEntry { inner: DirEntry("/home/robin/.cache/uv/.tmpgSvTCk/hexdump.py") }, DirEntry { inner: DirEntry("/home/robin/.cache/uv/.tmpgSvTCk/data") }, DirEntry { inner: DirEntry("/home/robin/.cache/uv/.tmpgSvTCk/PKG-INFO") }, DirEntry { inner: DirEntry("/home/robin/.cache/uv/.tmpgSvTCk/setup.py") }, DirEntry { inner: DirEntry("/home/robin/.cache/uv/.tmpgSvTCk/README.txt") }]
  Caused by: The top level of the archive must only contain a list directory, but it contains: [DirEntry { inner: DirEntry("/home/robin/.cache/uv/.tmpgSvTCk/__main__.py") }, DirEntry { inner: DirEntry("/home/robin/.cache/uv/.tmpgSvTCk/hexdump.py") }, DirEntry { inner: DirEntry("/home/robin/.cache/uv/.tmpgSvTCk/data") }, DirEntry { inner: DirEntry("/home/robin/.cache/uv/.tmpgSvTCk/PKG-INFO") }, DirEntry { inner: DirEntry("/home/robin/.cache/uv/.tmpgSvTCk/setup.py") }, DirEntry { inner: DirEntry("/home/robin/.cache/uv/.tmpgSvTCk/README.txt") }]
```

This PR removes the duplication and `DirEntry` internals so that the error message is easier to grasp:

```
$ uv pip install hexdump==3.3
error: Failed to download and build: hexdump==3.3
  Caused by: Failed to extract source distribution
  Caused by: The top level of the archive must only contain a list directory, but it contains: ["__main__.py", "hexdump.py", "data", "PKG-INFO", "setup.py", "README.txt"]
```

---

_Label `error messages` added by @zanieb on 2024-02-15 23:50_

---

_Review requested from @zanieb by @zanieb on 2024-02-15 23:50_

---

_Comment by @zanieb on 2024-02-15 23:50_

Thanks so much for contributing!

---

_@zanieb approved on 2024-02-16 00:03_

---

_Merged by @zanieb on 2024-02-16 00:03_

---

_Closed by @zanieb on 2024-02-16 00:03_

---

_Branch deleted on 2024-02-16 00:03_

---
