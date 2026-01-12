```yaml
number: 408
title: Accept more invalid version specifiers
type: issue
state: closed
author: konstin
labels:
  - bug
assignees: []
created_at: 2023-11-13T10:44:41Z
updated_at: 2023-11-13T20:20:59Z
url: https://github.com/astral-sh/uv/issues/408
synced_at: 2026-01-12T15:58:23Z
```

# Accept more invalid version specifiers

---

_@konstin_

https://pypi.org/simple/openpyxl/?format=application/vnd.pypi.simple.v1+json contains

```
>=3.6,
```

https://pypi.org/simple/pyzmq/?format=application/vnd.pypi.simple.v1+json contains

```
>=2.7,!=3.0*,!=3.1*,!=3.2*
```

`LenientVersionSpecifiers` should accept both

---

_Label `bug` added by @charliermarsh on 2023-11-13 14:32_

---

_Added to milestone `Initial release` by @charliermarsh on 2023-11-13 14:32_

---

_Comment by @konstin on 2023-11-13 16:16_

This is currently blocking `jupyter`:
```
puffin-dev failed
  Caused by: No solution found when resolving: jupyter
  Caused by: Received some unexpected JSON from https://pypi.org/simple/pyzmq/?format=application/vnd.pypi.simple.v1+json
  Caused by: Failed to parse version:
>=2.7,!=3.0*,!=3.1*,!=3.2*
      ^^^^^^
 at line 1 column 225673
```

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-13 20:01_

---

_Unassigned @charliermarsh by @charliermarsh on 2023-11-13 20:01_

---

_Closed by @charliermarsh on 2023-11-13 20:20_

---
