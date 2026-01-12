```yaml
number: 16492
title: UV not using overrides from build-system within pyproject.toml
type: issue
state: open
author: almmechanics
labels:
  - question
assignees: []
created_at: 2025-10-29T10:23:19Z
updated_at: 2025-10-29T11:36:23Z
url: https://github.com/astral-sh/uv/issues/16492
synced_at: 2026-01-12T16:02:32Z
```

# UV not using overrides from build-system within pyproject.toml

---

_@almmechanics_

### Summary

With legacy `pip` based pyprojects, it has been necessary to pin the versions of the tooling  `setuptools` and `setuptools-scm` due to breaking changes within those packages breaking our own projects

Our projects can be built with both `pip` and `uv` depending on the configuration, and not all have been migrated to `uv` tooling. This has been done by adding the following to the `pyproject.toml`

```toml
[build-system]
requires = ["setuptools==80.9.0", "setuptools-scm==8.3.1"]
build-backend = "setuptools.build_meta"
```

with a standard pip install  this will ensure the required version of `setuptools` and `setuptools-scm` are installed.

However, with the `uv pip install` this is ignored and `setuputils-65.5.0` is installed (from the uv's own requirements) but no warning is given that these settings have been ignored.

To make this compatible for both `uv` and `pip` we have to add the required version of `setuputils` into the `dependencies` section too.

```toml
dependencies = [
    "setuptools==80.9.0",
]
```

It would be great if `uv` would reflect the backwards compatibility for the `build-system` requirements instead of having to make the `pip` forwards compatible for `uv`. Additionally, having the same modules/version in the same file twice has the potential for creating compatibility issues in the future.



### Platform

Windows 11 x64

### Version

uv 0.9.5 (d5f39331a 2025-10-21)

### Python version

Python 3.10.11

---

_Label `bug` added by @almmechanics on 2025-10-29 10:23_

---

_Label `bug` removed by @konstin on 2025-10-29 11:34_

---

_Label `question` added by @konstin on 2025-10-29 11:34_

---

_Comment by @konstin on 2025-10-29 11:36_

uv does use the the versions in `build-system.requires`, but only for the isolated build environment, not for the runtime environment; usually setuptools isn't required for the runtime, and from Python 3.12 it's not included by default anymore. I'm surprised you're seeing different results with pip, which also default to build isolation in recent versions, are you setting any specific pip options?

---
