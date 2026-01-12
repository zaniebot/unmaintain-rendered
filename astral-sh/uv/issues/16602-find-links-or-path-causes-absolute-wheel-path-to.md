```yaml
number: 16602
title: "`find-links` or `path =` causes absolute wheel path to become relative path when creating lock file"
type: issue
state: open
author: marcschachtsiek
labels:
  - bug
assignees: []
created_at: 2025-11-05T17:17:24Z
updated_at: 2025-12-18T17:05:21Z
url: https://github.com/astral-sh/uv/issues/16602
synced_at: 2026-01-12T16:02:34Z
```

# `find-links` or `path =` causes absolute wheel path to become relative path when creating lock file

---

_@marcschachtsiek_

### Summary

This issue is effectively the complete inverse of [#6458](https://github.com/astral-sh/uv/issues/6458).

I am working on a project that is used in a cloud environment where we have some wheels in a shared folder (defined by an absolute path). Everyone has a space that they can use to work on projects so everyone may have a different repo directory and path to it (it becomes a problem when the `uv.lock` file is created in one that is fewer folders deep than the one it will be used in). There are multiple packages that have their wheels located in that shared wheels folder.

```toml
dependencies = [
    "package-in-wheels-folder>=1.0.0"
]

[tool.uv]
find-links = [
    "/shared_drive/some/folders/wheels"
]
```

When creating the `uv.lock` file, each of them will have their path made relative like:

```toml
[[package]]
name = "package-in-wheels-folder"
version = "1.0.0"
source = { path = "../../../../../../shared_drive/some/folders/wheels/<wheels_file>.whl" }
```

If someone now wants to recreate the environment with `uv sync --frozen` and happens to be in a deeper nested folder than this, it will fail. From a more shallow one this still happens to work since extra `..` will just stay in the base directory `/`.

It fails as expected given the `uv.lock` file with:

```
  × Failed to download `package-in-wheels-folder==1.0.0`
  ├─▶ Failed to read from the distribution cache
  ╰─▶ failed to query metadata of file
      `/remaining/bits/from/deeper/path/shared_drive/some/folders/wheels/<wheels_file>.whl`: No such file or directory (os error 2)
```

In this example, `/remaining/bits/from/deeper/path` is the part of the path that the repo is deeper than all of the `../` in the `uv.lock` file.

I realise this is quite an edge case, but nonetheless it would be useful if the `uv.lock` file respected the type of path from the `pyproject.toml` file, i.e. relative in `pyproject.toml` becomes relative in `uv.lock` (to keep [#6458](https://github.com/astral-sh/uv/issues/6458) resolved) and if it is absolute in `puproject.toml` then it should be absolute in `uv.lock` to resolve this issue. Or perhaps some default and explicit keyword to ensure it is clear.

Note: the same has also been tested by explicitly setting the `path` for the package and results in the same problem.
```toml
[tool.uv.sources]
package-in-wheels-folder = { path = "/shared_drive/some/folders/wheels/<wheels_file>.whl" }
```

### Platform

Linux 6.8.0 (AWS) x86_64 GNU/Linux

### Version

uv 0.9.7

### Python version

Python 3.10.12

---

_Label `bug` added by @marcschachtsiek on 2025-11-05 17:17_

---

_Comment by @marcschachtsiek on 2025-11-06 09:46_

Just found [#11832](https://github.com/astral-sh/uv/issues/11832). 

> [...] and lockfiles are intended to be committed.

This part of the first reply is why this is an issue here, someone creating it and someone else using it from a different location with the `--frozen` flag can lead to the issue above.

> This is a useful feature, in cases with networked drives (e.g. we have an existing pip wheel cache on a networked drive) in lab enviornments, in this case absolute paths are actually more portable

I agree with this final comment, perhaps there could be an keyword that can be used (so perhaps not `find-links`, but as a replacement for `path =`) to explicitly mark something as an absolute path. 

---

_Comment by @marcschachtsiek on 2025-11-06 09:51_

If this is currently the expected behaviour and on purpose, perhaps this issue is becoming more of a feature request, less of a bug report.

---

_Comment by @hanslovsky on 2025-11-06 15:08_

Thank you for opening this @marcschachtsiek. I have the exact same problem yesterday and was preparing a minimum example in preparation to open an issue, but you beat me to it. I am 100% interested in an existing solution, or to request this as a bug fix/feature, if it is not possible to preserve absolute paths in find-links at this time.

Note that I tried to set
```
source = { path = "/shared_drive/some/folders/wheels/<wheels_file>.whl" }
```
manually in `uv.lock`, but subsequent `uv sync` reverted that change to a relative path.

---

_Comment by @konstin on 2025-12-18 17:05_

The logic for relative/absolute paths is unfortunately complex because it touches a lot of parts, but I agree that we shouldn't turn explicit absolute paths to relative paths.

---
