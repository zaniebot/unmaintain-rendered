---
number: 515
title: Reflink obscures errors when copying directories
type: issue
state: closed
author: zanieb
labels:
  - bug
assignees: []
created_at: 2023-11-29T18:30:21Z
updated_at: 2023-12-01T18:53:29Z
url: https://github.com/astral-sh/uv/issues/515
synced_at: 2026-01-10T01:23:04Z
---

# Reflink obscures errors when copying directories

---

_Issue opened by @zanieb on 2023-11-29 18:30_

```
error: Failed to install: example-pkg-b @ git+https://github.com/pypa/sample-namespace-packages.git@df7530eeb8fa0cb7dbb8ecb28363e8e36bfa2f45#subdirectory=pkg_resources/pkg_b
  Caused by: Failed to reflink /Users/mz/eng/src/astral-sh/puffin/cache/wheels-v0/url/e893521e97fa8b9e/example_pkg_b-1-py3-none-any.whl/example_pkg to /Users/mz/eng/src/astral-sh/puffin/.direnv/python-3.11/lib/python3.11/site-packages/example_pkg
  Caused by: the source path is not an existing regular file
```

They rewrite the error message at

- https://github.com/cargo-bins/reflink-copy/blob/main/src/lib.rs#L61
- https://github.com/cargo-bins/reflink-copy/blob/main/src/lib.rs#L103-L112

meaning when we copy a directory and it fails, we get a meaningless error about the source not being a file.

With that code removed, we get a better error

```
error: Failed to install: example-pkg-b @ git+https://github.com/pypa/sample-namespace-packages.git@df7530eeb8fa0cb7dbb8ecb28363e8e36bfa2f45#subdirectory=pkg_resources/pkg_b
  Caused by: Failed to reflink /Users/mz/eng/src/astral-sh/puffin/cache/wheels-v0/url/e893521e97fa8b9e/example_pkg_b-1-py3-none-any.whl/example_pkg to /Users/mz/eng/src/astral-sh/puffin/.direnv/python-3.11/lib/python3.11/site-packages/example_pkg
  Caused by: File exists (os error 17)
```

---

_Comment by @zanieb on 2023-11-29 18:33_

Since macOS supports `clonefile` with directories, I'm unsure of the intent of this error transposition.

---

_Referenced in [astral-sh/uv#516](../../astral-sh/uv/pulls/516.md) on 2023-11-29 19:11_

---

_Comment by @konstin on 2023-11-29 19:30_

Upstream PR: https://github.com/cargo-bins/reflink-copy/pull/51

---

_Label `bug` added by @charliermarsh on 2023-11-30 01:54_

---

_Added to milestone `Initial release` by @charliermarsh on 2023-11-30 01:54_

---

_Comment by @charliermarsh on 2023-12-01 18:53_

I _think_ this is fixed now.

---

_Closed by @charliermarsh on 2023-12-01 18:53_

---
