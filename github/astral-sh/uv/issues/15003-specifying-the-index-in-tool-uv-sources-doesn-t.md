---
number: 15003
title: "Specifying the index in `tool.uv.sources` doesn't work with transitive dependencies"
type: issue
state: closed
author: jackrosenthal
labels:
  - bug
assignees: []
created_at: 2025-07-31T17:49:44Z
updated_at: 2025-08-01T01:56:34Z
url: https://github.com/astral-sh/uv/issues/15003
synced_at: 2026-01-07T13:12:19-06:00
---

# Specifying the index in `tool.uv.sources` doesn't work with transitive dependencies

---

_Issue opened by @jackrosenthal on 2025-07-31 17:49_

### Summary

When specifying a package in `tool.uv.sources`, the package's index isn't respected if the package is a transitive dependency.

Here's a minimal reproduction using a locally-hosted `pypiserver`:

```shellsession
$ mkdir ~/pypiserver
$ cd ~/pypiserver
$ pip download linetable
$ uvx --from pypiserver pypi-server run .
```

Now, we can verify we have a PyPI server running at `http://localhost:8080/simple`, and it has the `linetable` package available.

Next, let's set up a project to test with.

```shellsession
$ mkdir ~/test_project
$ cd ~/test_project
$ uv init
```

Next, we add to `pyproject.toml`:

```toml
[[tool.uv.index]]
name = "local"
url = "http://localhost:8080/simple"
explicit = true

[tool.uv.sources]
linetable = { index = "local" }
```

We should expect at this point, if we add a dependency which has `linetable` as a transitive dependency, we're going to get it from our `local` index.  But that's not the case.

```shellsession
$ uv add kajiki
$ cat uv.lock | grep -E 'name|source'
name = "kajiki"
source = { registry = "https://pypi.org/simple" }
    { name = "linetable" },
name = "linetable"
source = { registry = "https://pypi.org/simple" }
name = "test-project"
source = { virtual = "." }
    { name = "kajiki" },
requires-dist = [{ name = "kajiki", specifier = ">=1.0.2" }]
```

(notice `source` is set to `https://pypi.org/simple` above)

This gets resolved if we manually `uv add linetable`:

```shellsession
$ uv add linetable
$ cat uv.lock | grep -E 'name|source'
name = "kajiki"
source = { registry = "https://pypi.org/simple" }
    { name = "linetable" },
name = "linetable"
source = { registry = "http://localhost:8080/simple" }
name = "test-project"
source = { virtual = "." }
    { name = "kajiki" },
    { name = "linetable" },
    { name = "kajiki", specifier = ">=1.0.2" },
    { name = "linetable", specifier = ">=0.0.3", index = "http://localhost:8080/simple" },
```

Unless users are carefully inspecting `uv.lock`, they may not notice this and get the wrong package.

### Platform

Linux 6.15.7-arch1-1 x86_64 GNU/Linux

### Version

uv 0.8.3 (7e78f54e7 2025-07-24)

### Python version

Python 3.13.0

---

_Label `bug` added by @jackrosenthal on 2025-07-31 17:49_

---

_Comment by @zanieb on 2025-07-31 17:54_

Sources are currently never applied to transitive dependencies.

---

_Comment by @zanieb on 2025-07-31 17:55_

e.g., see https://github.com/astral-sh/uv/issues/8253

---

_Comment by @zanieb on 2025-07-31 17:55_

We're considering changing this though

---

_Closed by @charliermarsh on 2025-08-01 01:56_

---
