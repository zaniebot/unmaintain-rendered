```yaml
number: 900
title: Stub out DTLSsocket test
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/stub-out-dtlssocket
created_at: 2024-01-12T16:37:50Z
updated_at: 2024-01-15T09:16:33Z
url: https://github.com/astral-sh/uv/pull/900
synced_at: 2026-01-10T15:39:02Z
```

# Stub out DTLSsocket test

---

_Pull request opened by @konstin on 2024-01-12 16:37_

Replace the DTLSsocket test with a dummy package that does nothing but contain the build system specs that we need. This should speed up one of the slowest tests.

Part of #878

---

_Review requested from @zanieb by @konstin on 2024-01-12 16:38_

---

_Label `internal` added by @konstin on 2024-01-12 17:15_

---

_@charliermarsh approved on 2024-01-12 17:45_

Did you create this package?

---

_Comment by @konstin on 2024-01-12 17:50_

Yes, it's just a dummy

---

_Merged by @konstin on 2024-01-12 17:50_

---

_Closed by @konstin on 2024-01-12 17:50_

---

_Branch deleted on 2024-01-12 17:50_

---

_Comment by @zanieb on 2024-01-12 18:22_

@konstin Can you link to the package source in the test doc?

---

_Comment by @konstin on 2024-01-15 09:16_

**pyproject.toml**

```toml
[build-system]
requires = ["Cython<3", "setuptools", "wheel"]
```

**setup.py**

```python
from setuptools import setup

# Ensure that Cython was installed
# noinspection PyUnresolvedReferences
import Cython

setup(
    name='build-system-no-backend',
    version='0.1.0',
    install_requires=[],
)
```

**build_system_no_backend/__init__.py**

_Empty file_

---
