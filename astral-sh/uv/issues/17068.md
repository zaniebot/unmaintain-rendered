```yaml
number: 17068
title: "Index strategies are ignored with [required-]environments"
type: issue
state: open
author: konstin
labels: []
assignees: []
created_at: 2025-12-10T11:16:51Z
updated_at: 2025-12-10T11:16:51Z
url: https://github.com/astral-sh/uv/issues/17068
synced_at: 2026-01-10T03:11:35Z
```

# Index strategies are ignored with [required-]environments

---

_Issue opened by @konstin on 2025-12-10 11:16_

Continuing from https://github.com/astral-sh/uv/issues/17067: Adding `index-strategy = "unsafe-best-match"` or `index-strategy = "unsafe-first-match" doesn't help in the example below and don't pick up the source distribution.

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

Neither does combining index strategies and required environments, depending on the index order, we either fail or use the source distribution, while we should consistently pick the source distribution.

```toml
[tool.uv]
index-strategy = "unsafe-first-match"
required-environments = ["sys_platform == 'linux'"]
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
