```yaml
number: 16818
title: "`uv.lock` doesn't include hashes for path-based wheels"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2025-11-22T20:23:02Z
updated_at: 2025-11-26T01:05:59Z
url: https://github.com/astral-sh/uv/issues/16818
synced_at: 2026-01-12T16:02:38Z
```

# `uv.lock` doesn't include hashes for path-based wheels

---

_@charliermarsh_

Sometimes?

E.g.:

```
❯ uv add ~/Downloads/triton-3.3.1-cp312-cp312-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl --no-sync
Using CPython 3.14.0
Resolved 3 packages in 1.14s
```

Then:

```
version = 1
revision = 3
requires-python = ">=3.14"

[[package]]
name = "foo"
version = "0.1.0"
source = { virtual = "." }
dependencies = [
    { name = "triton" },
]

[package.metadata]
requires-dist = [{ name = "triton", path = "../../Downloads/triton-3.3.1-cp312-cp312-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl" }]

[[package]]
name = "setuptools"
version = "80.9.0"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/18/5d/3bf57dcd21979b887f014ea83c24ae194cfcd12b9e0fda66b957c69d1fca/setuptools-80.9.0.tar.gz", hash = "sha256:f36b47402ecde768dbfafc46e8e4207b4360c654f1f3bb84475f0a28628fb19c", size = 1319958, upload-time = "2025-05-27T00:56:51.443Z" }
wheels = [
    { url = "https://files.pythonhosted.org/packages/a3/dc/17031897dae0efacfea57dfd3a82fdd2a2aeb58e0ff71b77b87e44edc772/setuptools-80.9.0-py3-none-any.whl", hash = "sha256:062d34222ad13e0cc312a4c02d73f059e86a4acbfbdea8f8f76b28c99f306922", size = 1201486, upload-time = "2025-05-27T00:56:49.664Z" },
]

[[package]]
name = "triton"
version = "3.3.1"
source = { path = "../../Downloads/triton-3.3.1-cp312-cp312-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl" }
dependencies = [
    { name = "setuptools" },
]

[package.metadata]
requires-dist = [
    { name = "autopep8", marker = "extra == 'tests'" },
    { name = "cmake", marker = "extra == 'build'", specifier = ">=3.20" },
    { name = "isort", marker = "extra == 'tests'" },
    { name = "lit", marker = "extra == 'build'" },
    { name = "llnl-hatchet", marker = "extra == 'tests'" },
    { name = "matplotlib", marker = "extra == 'tutorials'" },
    { name = "numpy", marker = "extra == 'tests'" },
    { name = "pandas", marker = "extra == 'tutorials'" },
    { name = "pytest", marker = "extra == 'tests'" },
    { name = "pytest-forked", marker = "extra == 'tests'" },
    { name = "pytest-xdist", marker = "extra == 'tests'" },
    { name = "scipy", marker = "extra == 'tests'", specifier = ">=1.7.1" },
    { name = "setuptools", specifier = ">=40.8.0" },
    { name = "tabulate", marker = "extra == 'tutorials'" },
]
provides-extras = ["build", "tests", "tutorials"]
```

But sometimes they _do_ get included:

```
version = 1
revision = 3
requires-python = ">=3.14"

[[package]]
name = "foo"
version = "0.1.0"
source = { virtual = "." }
dependencies = [
    { name = "iniconfig" },
]

[package.metadata]
requires-dist = [{ name = "iniconfig", path = "../../Downloads/iniconfig-2.3.0-py3-none-any.whl" }]

[[package]]
name = "iniconfig"
version = "2.3.0"
source = { path = "../../Downloads/iniconfig-2.3.0-py3-none-any.whl" }
wheels = [
    { filename = "iniconfig-2.3.0-py3-none-any.whl", hash = "sha256:f631c04d2c48c52b84d0d0549c99ff3859c98df65b3101406327ecc7d53fbf12" },
]
```

---

_Label `bug` added by @charliermarsh on 2025-11-22 20:23_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-11-23 20:27_

---

_Comment by @charliermarsh on 2025-11-23 20:27_

I figured this out. The wheels get filtered out because they don’t match the required Python version, so it ends up emitting an invalid lockfile. We should error in the resolver.

---

_Closed by @charliermarsh on 2025-11-26 01:05_

---
