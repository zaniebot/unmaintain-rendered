```yaml
number: 17360
title: "`uv sync` doesn't run setup.py unless it is manually touched/modified"
type: issue
state: open
author: kunaltyagi
labels:
  - question
assignees: []
created_at: 2026-01-08T14:20:29Z
updated_at: 2026-01-14T09:31:23Z
url: https://github.com/astral-sh/uv/issues/17360
synced_at: 2026-01-14T10:34:33Z
```

# `uv sync` doesn't run setup.py unless it is manually touched/modified

---

_@kunaltyagi_

### Summary

I can not list extension files under `tool.setuptools.ext-modules` in `pyproject.toml` for uv to track, so I expect `uv sync` to take extra time on every call to see if `setup.py` needs to update any extension. However, even when there's work to be done, `uv sync` doesn't call `setup.py` unless I `touch`/modify it specifically.

Any solution/fix would be appreciated

### Platform

Linux and Windows

### Version

0.9.5

### Python version

3.12.12

---

_Label `bug` added by @kunaltyagi on 2026-01-08 14:20_

---

_Comment by @zanieb on 2026-01-08 14:40_

Please see https://docs.astral.sh/uv/reference/settings/#cache-keys

---

_Label `bug` removed by @zanieb on 2026-01-08 14:41_

---

_Label `question` added by @zanieb on 2026-01-08 14:41_

---

_Comment by @kunaltyagi on 2026-01-08 16:04_

I don't think `dir` is working as expected. Files changed under the directory don't trigger a call to `setup.py`

---

_Comment by @konstin on 2026-01-08 18:19_

Can you provide a minimal reproducible example for how it fails?

---

_Comment by @kunaltyagi on 2026-01-14 09:04_

Let me create one

---

_Comment by @kunaltyagi on 2026-01-14 09:31_

<details>
<summary>pyproject.toml</summary>

```toml
[project]
name = "test_uv"
version = "0.1.0"
description = "Test UV"
readme = "README.md"
requires-python = "==3.12.*"
dependencies = [
]
classifiers = ["Private :: Do Not Upload"]

[build-system]
requires = [
  "cmake",
  "setuptools",
]
build-backend = "setuptools.build_meta"

[tool.setuptools]
packages = ["test_uv"]

[tool.setuptools.package-dir]
test_uv = "src/test_uv"

[tool.uv]
cache-keys = [{ dir = "csrc" }, {file = "setup.py"}, { file = "pyproject.toml" }]
```

</details>

<details>
<summary>setup.py</summary>

```py
from setuptools import Extension, setup

print("****** Start setup.py ********")

setup(
    ext_modules=[
        Extension(
            name="foo",
            sources=["csrc/foo.c"],
        ),
    ]
)

print("****** End setup.py ********")
```

</details>

<details>
<summary>Setup the structure</summary>

```bash
touch README.md
mkdir -p src/test_uv
touch src/test_uv/__init__.py
mkdir -p csrc
```

</details>

```sh
echo "" > csrc/foo.c # to give a simple target
uv sync # everything works
echo "a" > csrc/foo.c # should make it not compile, BUT
uv sync # no error in ~0.1ms
touch setup.py # uv will try to compile it now
uv sync  # see errors
```

Sample output:
```
k@machine:~/test_uv$ uv sync
Resolved 1 package in 1ms
      Built test-uv @ file:///home/kuntyagi/test_uv
Prepared 1 package in 773ms
Uninstalled 1 package in 0.38ms
Installed 1 package in 1ms
 ~ test-uv==0.1.0 (from file:///home/kuntyagi/test_uv)
k@machine:~/test_uv$ echo "a" > csrc/foo.c # should make it not compile, BUT
k@machine:~/test_uv$ uv sync # no error in ~0.1ms
Resolved 1 package in 1ms
Audited 1 package in 0.13ms
```

---
