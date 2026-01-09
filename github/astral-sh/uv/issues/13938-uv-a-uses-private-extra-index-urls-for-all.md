---
number: 13938
title: uv a uses private extra index URLs for all packages in the uv.lock instead of the main index URL
type: issue
state: open
author: Ark-kun
labels:
  - bug
assignees: []
created_at: 2025-06-10T02:39:26Z
updated_at: 2025-06-13T12:21:19Z
url: https://github.com/astral-sh/uv/issues/13938
synced_at: 2026-01-07T13:12:18-06:00
---

# uv a uses private extra index URLs for all packages in the uv.lock instead of the main index URL

---

_Issue opened by @Ark-kun on 2025-06-10 02:39_

### Summary

Config:

```
% cat ~/.config/uv/uv.toml 
index-url = "https://pypi.python.org/simple"
extra-index-url = ["https://token:XXX@pkgs.shopify.io/basic/data/python/simple/"]

index-strategy = "unsafe-best-match"
```

```
% uv init    
Initialized project `test2`
(backend-test-python) ark@ test2 % uv add six -v
DEBUG uv 0.7.2 (481d05d8d 2025-04-30)
DEBUG Found project root: `/Users/ark/src/github.com/Shopify/test2`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `/Users/ark/src/github.com/Shopify/test2`
DEBUG Reading Python requests from version file at `/Users/ark/src/github.com/Shopify/test2/.python-version`
DEBUG Using Python request `3.12` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
DEBUG Searching for Python 3.12 in managed installations or search path
DEBUG Searching for managed installations at `/Users/ark/.local/share/uv/python`
DEBUG Found managed installation `cpython-3.12.9-macos-aarch64-none`
DEBUG Found `cpython-3.12.9-macos-aarch64-none` at `/Users/ark/.local/share/uv/python/cpython-3.12.9-macos-aarch64-none/bin/python3.12` (managed installations)
Using CPython 3.12.9
Creating virtual environment at: .venv
DEBUG Assessing Python executable as base candidate: /Users/ark/.local/share/uv/python/cpython-3.12.9-macos-aarch64-none/bin/python3.12
DEBUG Using base executable for virtual environment: /Users/ark/.local/share/uv/python/cpython-3.12.9-macos-aarch64-none/bin/python3.12
DEBUG Released lock at `/var/folders/_9/dl_tfx9169q_wbnqf639bn6r0000gn/T/uv-7868c7ccaf7ba2e4.lock`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: test2 @ file:///Users/ark/src/github.com/Shopify/test2
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.12.9
DEBUG Solving with target Python version: >=3.12
DEBUG Adding direct dependency: test2*
DEBUG Searching for a compatible version of test2 @ file:///Users/ark/src/github.com/Shopify/test2 (*)
DEBUG Adding direct dependency: six*
DEBUG Found fresh response for: https://pkgs.shopify.io/basic/data/python/simple/six/
DEBUG Found fresh response for: https://pypi.python.org/simple/six/
DEBUG Searching for a compatible version of six (*)
DEBUG Selecting: six==1.17.0 [compatible] (six-1.17.0-py2.py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b7/ce/149a00dd41f10bc29e5921b496af8b574d8413afcd5e30dfa0ed46c2cc5e/six-1.17.0-py2.py3-none-any.whl.metadata
DEBUG Tried 2 versions: six 1, test2 1
DEBUG all marker environments resolution took 0.001s
Resolved 2 packages in 2ms
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: test2 @ file:///Users/ark/src/github.com/Shopify/test2
DEBUG No workspace root found, using project root
DEBUG Ignoring existing lockfile due to mismatched requirements for: `test2==0.1.0`
  Requested: {Requirement { name: PackageName("six"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "1.17.0" }]), index: None, conflict: None }, origin: None }}
  Existing: {Requirement { name: PackageName("six"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([]), index: None, conflict: None }, origin: None }}
DEBUG Solving with installed Python version: 3.12.9
DEBUG Solving with target Python version: >=3.12
DEBUG Adding direct dependency: test2*
DEBUG Searching for a compatible version of test2 @ file:///Users/ark/src/github.com/Shopify/test2 (*)
DEBUG Adding direct dependency: six>=1.17.0
DEBUG Searching for a compatible version of six (>=1.17.0)
DEBUG Selecting: six==1.17.0 [preference] (six-1.17.0-py2.py3-none-any.whl)
DEBUG Tried 2 versions: six 1, test2 1
DEBUG all marker environments resolution took 0.000s
DEBUG Using request timeout of 30s
DEBUG Identified uncached distribution: six==1.17.0
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b7/ce/149a00dd41f10bc29e5921b496af8b574d8413afcd5e30dfa0ed46c2cc5e/six-1.17.0-py2.py3-none-any.whl
Prepared 1 package in 1ms
Installed 1 package in 2ms
 + six==1.17.0
```

Resulting uv.lock
```
version = 1
revision = 2
requires-python = ">=3.12"

[[package]]
name = "six"
version = "1.17.0"
source = { registry = "https://pkgs.shopify.io/basic/data/python/simple/" }
sdist = { url = "https://files.pythonhosted.org/packages/94/e7/b2c673351809dca68a0e064b6af791aa332cf192da575fd474ed7d6f16a2/six-1.17.0.tar.gz", hash = "sha256:ff70335d468e7eb6ec65b95b99d3a2836546063f63acc5171de367e834932a81", size = 34031, upload-time = "2024-12-04T17:35:28.174Z" }
wheels = [
    { url = "https://files.pythonhosted.org/packages/b7/ce/149a00dd41f10bc29e5921b496af8b574d8413afcd5e30dfa0ed46c2cc5e/six-1.17.0-py2.py3-none-any.whl", hash = "sha256:4721f391ed90541fddacab5acf947aa0d3dc7d27b2e1e8eda2be8970586c3274", size = 11050, upload-time = "2024-12-04T17:35:26.475Z" },
]

[[package]]
name = "test2"
version = "0.1.0"
source = { virtual = "." }
dependencies = [
    { name = "six" },
]

[package.metadata]
requires-dist = [{ name = "six", specifier = ">=1.17.0" }]
```

Expected:
```
source = { registry = "https://pypi.python.org/simple/" }
```
Actual:
```
source = { registry = "https://pkgs.shopify.io/basic/data/python/simple/" }
```

### Platform

macOS 15 arm64

### Version

uv 0.7.2 (481d05d8d 2025-04-30)

### Python version

Python 3.12.9

---

_Label `bug` added by @Ark-kun on 2025-06-10 02:39_

---

_Assigned to @jtfmumm by @konstin on 2025-06-10 09:40_

---

_Comment by @jtfmumm on 2025-06-10 15:52_

I've been investigating this issue. Testing with a GitLab package registry (which will internally fall back to PyPI if a package is not found), uv does write the GitLab registry URL to the lockfile as the source just as in your case. Could it be that your registry endpoint through Shopify is either directly using PyPI or falling back to it?

---

_Comment by @konstin on 2025-06-10 16:18_

Could you look at the html of e.g. https://pkgs.shopify.io/basic/data/python/simple/ (with credentials), are the URLs there pointing to shopify or PyPI?

---

_Comment by @nichmor on 2025-06-13 12:21_

> I've been investigating this issue. Testing with a GitLab package registry (which will internally fall back to PyPI if a package is not found), uv does write the GitLab registry URL to the lockfile as the source just as in your case. Could it be that your registry endpoint through Shopify is either directly using PyPI or falling back to it?

For those curious minds that also had this cross-related problem, please verify that your private registry does not fall back by default to pypi. In my case, https://github.com/pypiserver/pypiserver was falling back to the pypi server indeed.

---
