```yaml
number: 15190
title: uv sync does not switch from editable to plain install when run with --no-sources
type: issue
state: closed
author: per11235813
labels:
  - bug
assignees: []
created_at: 2025-08-09T13:48:19Z
updated_at: 2025-09-29T05:28:32Z
url: https://github.com/astral-sh/uv/issues/15190
synced_at: 2026-01-12T16:02:05Z
```

# uv sync does not switch from editable to plain install when run with --no-sources

---

_@per11235813_

### Summary

First of all, thanks for `uv`. The tool is just awesome, but I do have a problem with the `--no-source` option

Using the pyproject.toml below 

- Running `uv sync --no-sources`
    - `poethepoet` is installed as a package. Perfect
- Then run `uv sync`
    -  `poethepoet` is added as editable instaed of a package. Perfect
- Then run `uv sync --no-sources` again
    - No change are made
    - `poethepoet` is still editable, whihc is not so good.
    - I would expect re-running `uv sync --no-sources` would install `poethepoet` i as a package instead of editable, so that running `uv sync --no-sources` would allways give the same result. 

Example:
```bash
$ uv sync --no-sources  # installed as package ✅
 + pastel==0.2.1
 + poethepoet==0.35.0
 + pyyaml==6.0.2
 + test-no-sources==0.0.1 (from file:///D:/repo/psje/test-no-sources)

$ uv sync               # installed as editable ✅
 - poethepoet==0.35.0
 + poethepoet==0.35.0 (from file:///D:/repo/psje/poethepoet)

$ uv sync --no-sources  # no changes made ❌
$
```

Pyproject.toml:
```toml
[project]
name = "test_no_sources"
version = "0.0.1"

dependencies = ["poethepoet"]

[tool.uv.sources]
poethepoet = { path="../poethepoet", editable=true }

[build-system]
requires = ["setuptools>=67"]
build-backend = "setuptools.build_meta"
```


### Platform

Win 11; MINGW64_NT-10.0-22631 3.6.3-7674c51e.x86_64 x86_64 Msys

### Version

uv 0.8.8 (9a54754b0 2025-08-08)

### Python version

3.12.10 (also tested on 3.10)

---

_Label `bug` added by @per11235813 on 2025-08-09 13:48_

---

_Comment by @zanieb on 2025-08-09 14:52_

Interesting, I can reproduce. Sounds like a bug indeed.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-09-14 02:06_

---

_Closed by @charliermarsh on 2025-09-27 21:48_

---

_Comment by @per11235813 on 2025-09-29 05:28_

Thanks a lot, `uv` is fantastic!

---
