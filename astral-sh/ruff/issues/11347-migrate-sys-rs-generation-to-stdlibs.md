```yaml
number: 11347
title: "Migrate `sys.rs` generation to `stdlibs`"
type: issue
state: closed
author: charliermarsh
labels:
  - good first issue
  - internal
assignees: []
created_at: 2024-05-09T00:55:23Z
updated_at: 2024-05-13T01:21:53Z
url: https://github.com/astral-sh/ruff/issues/11347
synced_at: 2026-01-12T15:54:51Z
```

# Migrate `sys.rs` generation to `stdlibs`

---

_@charliermarsh_

This was brought to my attention via email. We have a [script](https://github.com/astral-sh/ruff/blob/56b4c47d74aecf9091890ffb8438c53f5822bb30/scripts/generate_known_standard_library.py) to generate https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_stdlib/src/sys.rs, and it does this by polling (e.g.) https://docs.python.org/3.10/objects.inv.

There's a nice Python library we can use instead of `objects.inv`, which (IIUC) catches a bunch of other modules that aren't present there (e.g., `_ssl`): https://github.com/omnilib/stdlibs

---

_Label `internal` added by @charliermarsh on 2024-05-09 00:55_

---

_Label `good first issue` added by @charliermarsh on 2024-05-09 00:55_

---

_Closed by @charliermarsh on 2024-05-13 01:21_

---
