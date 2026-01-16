```yaml
number: 17508
title: "recover default source when no extras are selected in [tool.uv.sources]"
type: issue
state: open
author: bkmi
labels:
  - question
assignees: []
created_at: 2026-01-16T00:30:55Z
updated_at: 2026-01-16T00:30:55Z
url: https://github.com/astral-sh/uv/issues/17508
synced_at: 2026-01-16T01:04:13Z
```

# recover default source when no extras are selected in [tool.uv.sources]

---

_@bkmi_

### Question

```toml
[tool.uv.sources]
fairchem-core = [
    { index = "pypi" },
    { git = "https://github.com/facebookresearch/fairchem", subdirectory = "packages/fairchem-core/", branch = "fairchem_core-2.13.0-numpy_unpinned", extra = "ccdc" },
]

[[tool.uv.index]]
name = "pypi"
url = "https://pypi.org/simple"
```

When I run

```bash
uv sync
```

the installed version is the github branch. I want it to install the version from `pypi`. Generally, I want to default to the default source whenever `extra` does not match anything.

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @bkmi on 2026-01-16 00:30_

---
