---
number: 9673
title: Wheel selection does not order by build tag correctly
type: issue
state: closed
author: jonded94
labels:
  - bug
assignees: []
created_at: 2024-12-06T10:54:45Z
updated_at: 2024-12-06T13:24:27Z
url: https://github.com/astral-sh/uv/issues/9673
synced_at: 2026-01-07T13:12:18-06:00
---

# Wheel selection does not order by build tag correctly

---

_Issue opened by @jonded94 on 2024-12-06 10:54_

Given this [issue](https://github.com/astral-sh/uv/issues/3779) I would have assumed that `uv` is capable to choose the most recent built tag version of a package whenever there are multiple versions of it.

Atleast with an internal package registry that doesn't seem to work for me. I'm using  Nexus 3.70.3-01 & uv 0.5.6.

**Building two wheels with different build tags**

Given this build command (I built with [scitkit-build-core](https://scikit-build-core.readthedocs.io/en/latest/configuration.html#customizing-the-output-wheel))
```
uv build --config-settings="wheel.build-tag=[number]" --wheel
```
, I build a wheel with a build tag of either 141 or 148 which just contains a function returning the build tag which I chose when building the wheel:
```
__all__ = ["version"]

def version() -> int:
    return 141  # or 148, respectively
```

**Installing wheel**

Now I create another testproject that depends on the testproject and create a lock file:
```
$ uv add --index https://nexus.[...]/repository/pip/simple/ testproj==0.0.1
$ cat uv.lock
version = 1
requires-python = "==3.10.*"

[[package]]
name = "testproj"
version = "0.0.1"
source = { registry = "https://nexus.[...]/repository/pip/simple/" }
wheels = [
    { url = "https://nexus.[...]/repository/pip/packages/testproj/0.0.1/testproj-0.0.1-141-cp310-cp310-linux_x86_64.whl", hash = "..." },
    { url = "https://nexus.[...]/repository/pip/packages/testproj/0.0.1/testproj-0.0.1-148-cp310-cp310-linux_x86_64.whl", hash = "..." },
]

[[package]]
name = "new-testproj"
version = "0.1.0"
source = { virtual = "." }
dependencies = [
    { name = "testproj" },
]

[package.metadata]
requires-dist = [{ name = "testproj", specifier = "==0.0.1" }]
```

As one can see, the `uv.lock` file includes both wheels that have different build tags.

**Testing which version was chosen**

Unfortunately though, it seems that `uv` now always chooses to install the package with the `-141` build tag:

```
$ uv run python -c "import testproj; print(testproj.version())"
141
$ grep Build .venv/lib/python3.10/site-packages/testproj-0.0.1.dist-info/WHEEL
Build: 141
```

I would have expected that it sorts the build tags alphanumerically and thereby would result in using the `-148` build tag wheel. This is how `poetry` works at least, and this blocks us from switching from `poetry` to `uv` unfortunately.

Is this intended behaviour?

---

_Comment by @charliermarsh on 2024-12-06 11:51_

Fairly certain that we handle this correctly in some places since I've fixed it before, but perhaps not in `uv.lock`? I'll look. We should be sorting by build tag, yes.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-06 11:51_

---

_Label `bug` added by @charliermarsh on 2024-12-06 12:09_

---

_Comment by @charliermarsh on 2024-12-06 12:23_

Confirmed bug, I'll fix it now, thanks.

---

_Referenced in [astral-sh/uv#9677](../../astral-sh/uv/pulls/9677.md) on 2024-12-06 12:29_

---

_Closed by @charliermarsh on 2024-12-06 12:40_

---

_Closed by @charliermarsh on 2024-12-06 12:40_

---

_Comment by @charliermarsh on 2024-12-06 13:24_

Some additional test coverage in https://github.com/astral-sh/uv/pull/9680.

---
