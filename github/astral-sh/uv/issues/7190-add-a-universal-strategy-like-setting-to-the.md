---
number: 7190
title: "Add a `--universal-strategy`-like setting to the resolver"
type: issue
state: closed
author: charliermarsh
labels:
  - resolver
assignees: []
created_at: 2024-09-08T15:13:29Z
updated_at: 2024-12-13T21:05:08Z
url: https://github.com/astral-sh/uv/issues/7190
synced_at: 2026-01-07T13:12:17-06:00
---

# Add a `--universal-strategy`-like setting to the resolver

---

_Issue opened by @charliermarsh on 2024-09-08 15:13_

In the universal resolver, we prioritize solving for as few versions as possible. Sometimes, users instead want to get the _newest_ possible version for every fork (e.g., they'd prefer getting the newest versions of `foo` for Python 3.10+, even if it means they have two versions of `foo` in the lockfile, one for Python 3.9 and earlier, and one for Python 3.10 and later).

(`--universal-strategy` is obviously a bad name.)

One implementation of this would be to ignore preferences from other forks.


---

_Label `resolver` added by @charliermarsh on 2024-09-08 15:13_

---

_Referenced in [astral-sh/uv#7551](../../astral-sh/uv/issues/7551.md) on 2024-09-20 02:52_

---

_Comment by @henryiii on 2024-10-29 17:01_

I'd even think this might be a nice default. Currently, the following simple example:

```console
$ mkdir example
$ cd example
$ cat > pyproject.toml << EOL
[project]
name = "example"
version = "0.1"
requires-python = ">=3.8"
dependencies = ["numpy"]
EOL
$ uv run python
Resolved 2 packages in 7ms
error: Failed to prepare distributions
  Caused by: Failed to download and build `numpy==1.24.4`
  Caused by: Build backend failed to determine requirements with `build_wheel()` (exit status: 1)

[stderr]
Traceback (most recent call last):
  File "<string>", line 8, in <module>
  File "/Users/henryschreiner/.cache/uv/builds-v0/.tmpjYjj2d/lib/python3.12/site-packages/setuptools/__init__.py", line 10, in <module>
    import distutils.core
ModuleNotFoundError: No module named 'distutils'
  Caused by: distutils was removed from the standard library in Python 3.12. Consider adding a constraint (like `numpy >1.24.4`) to avoid building a version of numpy that depends on distutils.
```

will break on Python 3.12 and newer. Any package that supports a narrower range than you do (and packages should be allowed to stop supporting old Python versions) will cause you do never get updates, and updates are often required for new Python versions.

This is one of the biggest drawbacks of a universal solver, as most users starting out don't realize that "dependencies = ["numpy"]` isn't going to give them the latest version on the version of Python they are running. I think this would really reduce the friction when starting out.

See https://github.com/astral-sh/uv/issues/8492 also.

---

_Referenced in [astral-sh/uv#8492](../../astral-sh/uv/issues/8492.md) on 2024-10-29 17:04_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-29 17:18_

---

_Referenced in [astral-sh/uv#8686](../../astral-sh/uv/pulls/8686.md) on 2024-10-29 20:56_

---

_Referenced in [astral-sh/uv#8978](../../astral-sh/uv/issues/8978.md) on 2024-11-10 10:42_

---

_Referenced in [astral-sh/uv#9515](../../astral-sh/uv/issues/9515.md) on 2024-11-29 19:22_

---

_Referenced in [astral-sh/uv#9868](../../astral-sh/uv/pulls/9868.md) on 2024-12-13 14:16_

---

_Closed by @charliermarsh on 2024-12-13 21:05_

---

_Referenced in [astral-sh/uv#9988](../../astral-sh/uv/issues/9988.md) on 2024-12-18 03:35_

---
