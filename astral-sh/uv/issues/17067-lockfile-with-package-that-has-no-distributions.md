---
number: 17067
title: Lockfile with package that has no distributions with environments
type: issue
state: open
author: konstin
labels: []
assignees: []
created_at: 2025-12-10T11:14:39Z
updated_at: 2025-12-11T15:29:29Z
url: https://github.com/astral-sh/uv/issues/17067
synced_at: 2026-01-10T01:26:13Z
---

# Lockfile with package that has no distributions with environments

---

_Issue opened by @konstin on 2025-12-10 11:14_

Using `tool.uv.environments` with an index that mismatches those environments creates a lockfile with an entry with no distributions. The resolution should instead fail.

```toml
[project]
name = "uv-reproducer"
version = "0.1.0"
requires-python = "==3.13.*"
dependencies = [
    "charset-normalizer==3.4.4",
]

[[tool.uv.index]]
url = "./platform-mismatch"
format = "flat"

[[tool.uv.index]]
url = "./default"
format = "flat"

[tool.uv]
environments = ["sys_platform == 'linux'"]
```

```toml
version = 1
revision = 3
requires-python = "==3.13.*"
resolution-markers = [
    "sys_platform == 'linux'",
]
supported-markers = [
    "sys_platform == 'linux'",
]

[[package]]
name = "charset-normalizer"
version = "3.4.4"
source = { registry = "platform-mismatch" }

[[package]]
name = "uv-reproducer"
version = "0.1.0"
source = { virtual = "." }
dependencies = [
    { name = "charset-normalizer", marker = "sys_platform == 'linux'" },
]

[package.metadata]
requires-dist = [{ name = "charset-normalizer", specifier = "==3.4.4" }]
```

To reproduce:

```
pushd default/
# Source file for charset-normalizer
curl -O https://files.pythonhosted.org/packages/13/69/33ddede1939fdd074bce5434295f38fae7136463422fe4fd3e0e89b98062/charset_normalizer-3.4.4.tar.gz
# And of course we need setuptools
curl -O https://files.pythonhosted.org/packages/a3/dc/17031897dae0efacfea57dfd3a82fdd2a2aeb58e0ff71b77b87e44edc772/setuptools-80.9.0-py3-none-any.whl
popd

# Works so long as you're not on a Windows on ARM host
pushd platform-mismatch/
curl -O https://files.pythonhosted.org/packages/af/8f/3ed4bfa0c0c72a7ca17f0380cd9e4dd842b09f664e780c13cff1dcf2ef1b/charset_normalizer-3.4.4-cp313-cp313-win_arm64.whl
popd
```

---

_Referenced in [astral-sh/uv#17068](../../astral-sh/uv/issues/17068.md) on 2025-12-10 11:16_

---

_Referenced in [astral-sh/uv#12721](../../astral-sh/uv/issues/12721.md) on 2025-12-10 11:18_

---

_Comment by @charliermarsh on 2025-12-11 15:19_

Is this only true of flat indexes?

---

_Comment by @konstin on 2025-12-11 15:29_

It works with a regular index too:

```toml
[project]
name = "debug"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = [
  "mkl==2025.3.0",
]

[[tool.uv.index]]
url = "https://download.pytorch.org/whl/"

[tool.uv]
environments = ["sys_platform == 'darwin'"]
```

```toml
[[package]]
name = "mkl"
version = "2025.3.0"
source = { registry = "https://download.pytorch.org/whl/" }
dependencies = [
    { name = "intel-openmp", marker = "sys_platform == 'darwin'" },
    { name = "onemkl-license", marker = "sys_platform == 'darwin'" },
    { name = "tbb", marker = "sys_platform == 'darwin'" },
]
```

`required-environments` correctly fails the resolution.

---
