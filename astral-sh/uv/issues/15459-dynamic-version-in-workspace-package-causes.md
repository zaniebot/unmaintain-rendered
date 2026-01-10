---
number: 15459
title: Dynamic version in workspace package causes failure to parse uv.lock when using --frozen
type: issue
state: closed
author: quartox
labels:
  - bug
assignees: []
created_at: 2025-08-22T17:04:43Z
updated_at: 2025-08-31T16:18:54Z
url: https://github.com/astral-sh/uv/issues/15459
synced_at: 2026-01-10T01:25:56Z
---

# Dynamic version in workspace package causes failure to parse uv.lock when using --frozen

---

_Issue opened by @quartox on 2025-08-22 17:04_

### Summary

I am using a multi-stage dockerfile. In one stage I want to do `uv sync --frozen --no-dev --no-install-workspace` to install all non-dev dependencies and then cache that docker layer. When I try to run this with my default uv.lock I get the following error for one of my workspace packages with dynamic versioning:

```
0.137 Failed to parse lockfile; ignoring locked requirements: TOML parse error at line 1230, column 1                                                                                             
0.137      |
0.137 1230 | [[package]]
0.137      | ^^^^^^^^^^^
0.137 missing field `version`
0.137 error: Unable to find lockfile at `uv.lock`. To create a lockfile, run `uv lock` or `uv sync`.
```

In the uv.lockfile it starts with 

```
name = "name-of-package"
source = { editable = "name-of-directory" }
```

My build system determines the version dynamically similar to [this issue](https://github.com/astral-sh/uv/issues/14037). If I run `uv sync` without the `--frozen` in the docker image build then it adds the version determined by the build system to the uv.lock file.

This requires putting the entire source directory into that layer of the dockerfile. This is sub-optimal because I do not want to re-build this layer every time I change my source file.

### Platform

macOS 15.6.1 arm

### Version

uv 0.8.13

### Python version

python 3.13

---

_Label `bug` added by @quartox on 2025-08-22 17:04_

---

_Comment by @charliermarsh on 2025-08-31 14:46_

Are you certain that you're using uv 0.8.13 in both contexts? That looks like the error you'd get if you were using an old version of uv. Version is an optional field in newer versions.

---

_Comment by @quartox on 2025-08-31 16:18_

Afaik I didn't change the docker file (it copies the uv binary from the astral 0.8.13 image) but I cannot reproduce today. Must have been wrong version within docker. Closing. 

---

_Closed by @quartox on 2025-08-31 16:18_

---
