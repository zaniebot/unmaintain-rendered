```yaml
number: 2313
title: Remove wheel for PEP 517 fallback builds
type: issue
state: closed
author: henryiii
labels:
  - enhancement
assignees: []
created_at: 2024-03-09T02:31:30Z
updated_at: 2024-03-10T23:34:38Z
url: https://github.com/astral-sh/uv/issues/2313
synced_at: 2026-01-12T15:58:37Z
```

# Remove wheel for PEP 517 fallback builds

---

_@henryiii_

Setuptools declares wheel as a wheel build dependency for PEP 517 builds already via a build hook. Both pip 24 (https://github.com/pypa/pip/pull/12449)  and build 1.1 (https://github.com/pypa/build/pull/716) have removed wheel from the default PEP 517 fallback environment. I think uv should do the same. 

There are a small collection of packages that `import wheel` inside setup.py directly, which is discouraged by `wheel` (as there's no official pubic API), and these packages should either declare wheel explicitly as a dependency in pyproject.toml, protect the import, or use setuptool's functionality to get a build class without importing it.

---

_Label `enhancement` added by @charliermarsh on 2024-03-09 14:43_

---

_Comment by @charliermarsh on 2024-03-09 14:44_

Any idea if there are known packages that won't build under this?

---

_Comment by @henryiii on 2024-03-10 05:54_

They also won't work with pip or build. :) The most recent issue was one with pyodide https://github.com/pyodide/pyodide/discussions/4593 - the user forgot to copy the pyproject.toml into the docker container when building. :) Another: https://github.com/pypa/build/issues/750. I think pip took the brunt of this, though, as pip 24 came out before build 1.1, and people using build tend to be more likely to have pyproject.tomls, etc., so looking around in pip's issue tracker might be helpful.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-10 16:16_

---

_Closed by @charliermarsh on 2024-03-10 23:34_

---
