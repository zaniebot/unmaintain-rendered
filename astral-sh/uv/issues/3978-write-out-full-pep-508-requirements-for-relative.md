```yaml
number: 3978
title: Write out full PEP 508 requirements for relative paths and editables
type: issue
state: open
author: charliermarsh
labels:
  - compatibility
assignees: []
created_at: 2024-06-03T02:00:20Z
updated_at: 2024-06-03T02:01:10Z
url: https://github.com/astral-sh/uv/issues/3978
synced_at: 2026-01-12T15:58:47Z
```

# Write out full PEP 508 requirements for relative paths and editables

---

_@charliermarsh_

In `pip compile`, we take special care to write out editables and relative paths as unnamed URLs. But uv supports PEP 508 requirements for editables, and allows relative URLs too. Should we just write them out in full form? The output `requirements.txt` won't work with pip, since pip does not yet support PEP 508 for editables, so we'd need to have a `--pip-compat` flag or similar to allow that behavior.


---

_Label `compatibility` added by @charliermarsh on 2024-06-03 02:00_

---

_Comment by @charliermarsh on 2024-06-03 02:01_

Concretely, see `RequirementsTxtDist::to_requirements_txt`. In theory, we shouldn't need that special-casing. We can write `-e` for an editable, then write a PEP 508 requirement in either case.

---
