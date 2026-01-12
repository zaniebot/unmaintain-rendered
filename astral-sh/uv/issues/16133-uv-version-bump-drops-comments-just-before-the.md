```yaml
number: 16133
title: "`uv version --bump ...` drops comments just before the version"
type: issue
state: closed
author: khneal
labels:
  - bug
  - good first issue
assignees: []
created_at: 2025-10-06T04:21:00Z
updated_at: 2025-10-09T16:45:07Z
url: https://github.com/astral-sh/uv/issues/16133
synced_at: 2026-01-12T16:02:24Z
```

# `uv version --bump ...` drops comments just before the version

---

_@khneal_

### Summary

I noticed that using `uv version --bump ...` will drop any comments in pyproject.toml on lines immediately preceding the "version = ..." line.

For example, running `uv version --bump patch` with this pyproject.toml:
```pyproject.toml
# hello
[project]
# hello
name = "hello-world"
# hello
version = "0.1.0"
# hello
description = "Add your description here"
# hello
readme = "README.md"
# hello
requires-python = ">=3.13"
# hello
dependencies = []
# hello
```

Will result in this:

```pyproject.toml
# hello
[project]
# hello
name = "hello-world"
version = "0.1.1"
# hello
description = "Add your description here"
# hello
readme = "README.md"
# hello
requires-python = ">=3.13"
# hello
dependencies = []
# hello
```


### Platform

macOS 15 arm64

### Version

uv 0.8.22 (Homebrew 2025-09-23)

### Python version

Python 3.13.7 (main, Sep 18 2025, 22:52:34) [Clang 20.1.4 ]

---

_Label `bug` added by @khneal on 2025-10-06 04:21_

---

_Label `good first issue` added by @konstin on 2025-10-06 09:11_

---

_Comment by @konstin on 2025-10-06 09:12_

We need to preserve the decor when changing the version.

---

_Comment by @assadyousuf on 2025-10-06 18:45_

Have a fix for this: https://github.com/astral-sh/uv/pull/16141

---

_Closed by @konstin on 2025-10-09 16:07_

---

_Comment by @khneal on 2025-10-09 16:45_

Thank you @assadyousuf and @konstin!
This will help us retain comments for developers that are not as familiar with pyproject.toml.

---
