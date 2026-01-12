```yaml
number: 10789
title: Avoid deserialization error for paths above the root
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/lax
created_at: 2025-01-20T21:02:14Z
updated_at: 2025-01-20T21:36:20Z
url: https://github.com/astral-sh/uv/pull/10789
synced_at: 2026-01-12T16:09:29Z
```

# Avoid deserialization error for paths above the root

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/10777

## Test Plan

Copied over this lockfile:

```toml
version = 1
requires-python = ">=3.12"
resolution-markers = [
    "sys_platform == 'win32'",
    "sys_platform != 'win32'",
]

[[package]]
name = "pyasn1"
version = "0.6.1"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/ba/e9/01f1a64245b89f039897cb0130016d79f77d52669aae6ee7b159a6c4c018/pyasn1-0.6.1.tar.gz", hash = "sha256:6f580d2bdd84365380830acf45550f2511469f673cb4a5ae3857a3170128b034", size = 145322 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/c8/f1/d6a797abb14f6283c0ddff96bbdd46937f64122b8c925cab503dd37f8214/pyasn1-0.6.1-py3-none-any.whl", hash = "sha256:0d632f46f2ba09143da3a8afe9e33fb6f92fa2320ab7e886e2d0f7672af84629", size = 83135 },
]

[[package]]
name = "pyasn1-modules"
version = "0.4.1"
source = { registry = "https://pypi.org/simple" }
dependencies = [
    { name = "pyasn1" },
]
sdist = { url = "https://files.pythonhosted.org/packages/1d/67/6afbf0d507f73c32d21084a79946bfcfca5fbc62a72057e9c23797a737c9/pyasn1_modules-0.4.1.tar.gz", hash = "sha256:c28e2dbf9c06ad61c71a075c7e0f9fd0f1b0bb2d2ad4377f240d33ac2ab60a7c", size = 310028 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/77/89/bc88a6711935ba795a679ea6ebee07e128050d6382eaa35a0a47c8032bdc/pyasn1_modules-0.4.1-py3-none-any.whl", hash = "sha256:49bfa96b45a292b711e986f222502c1c9a5e1f4e568fc30e2574a6c7d07838fd", size = 181537 },
]

[[package]]
name = "python-ldap"
version = "3.4.4"
source = { registry = "https://pypi.org/simple" }
resolution-markers = [
    "sys_platform != 'win32'",
]
dependencies = [
    { name = "pyasn1", marker = "sys_platform != 'win32'" },
    { name = "pyasn1-modules", marker = "sys_platform != 'win32'" },
]
sdist = { url = "https://files.pythonhosted.org/packages/fd/8b/1eeb4025dc1d3ac2f16678f38dec9ebdde6271c74955b72db5ce7a4dbfbd/python-ldap-3.4.4.tar.gz", hash = "sha256:7edb0accec4e037797705f3a05cbf36a9fde50d08c8f67f2aef99a2628fab828", size = 377889 }

[[package]]
name = "python-ldap"
version = "3.4.4"
source = { path = "../../../uti/Python/python_ldap-3.4.4-cp312-cp312-win_amd64.whl" }
resolution-markers = [
    "sys_platform == 'win32'",
]
dependencies = [
    { name = "pyasn1", marker = "sys_platform == 'win32'" },
    { name = "pyasn1-modules", marker = "sys_platform == 'win32'" },
]
wheels = [
    { filename = "python_ldap-3.4.4-cp312-cp312-win_amd64.whl", hash = "sha256:94d2ca2b3ced81c9d89aa5c79d4965d03053e1ffdcfae73e9fac85d25b692e85" },
]

[package.metadata]
requires-dist = [
    { name = "pyasn1", specifier = ">=0.3.7" },
    { name = "pyasn1-modules", specifier = ">=0.1.5" },
]

[[package]]
name = "uv-test"
version = "1.0"
source = { virtual = "." }
dependencies = [
    { name = "python-ldap", version = "3.4.4", source = { registry = "https://pypi.org/simple" }, marker = "sys_platform != 'win32'" },
    { name = "python-ldap", version = "3.4.4", source = { path = "../../../uti/Python/python_ldap-3.4.4-cp312-cp312-win_amd64.whl" }, marker = "sys_platform == 'win32'" },
]

[package.metadata]
requires-dist = [
    { name = "python-ldap", marker = "sys_platform != 'win32'" },
    { name = "python-ldap", marker = "sys_platform == 'win32'", path = "../../../../../../../../../../../../uti/Python/python_ldap-3.4.4-cp312-cp312-win_amd64.whl" },
]
```

Verified that `cargo run sync --frozen` installs `python-ldap` from PyPI, without erroring.


---

_Label `bug` added by @charliermarsh on 2025-01-20 21:02_

---

_Merged by @charliermarsh on 2025-01-20 21:36_

---

_Closed by @charliermarsh on 2025-01-20 21:36_

---

_Branch deleted on 2025-01-20 21:36_

---
