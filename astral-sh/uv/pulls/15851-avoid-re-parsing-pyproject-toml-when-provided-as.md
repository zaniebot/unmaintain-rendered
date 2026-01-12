```yaml
number: 15851
title: "Avoid re-parsing `pyproject.toml` when provided as a source"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/pyproj
created_at: 2025-09-15T00:38:56Z
updated_at: 2025-09-15T14:07:40Z
url: https://github.com/astral-sh/uv/pull/15851
synced_at: 2026-01-12T16:11:58Z
```

# Avoid re-parsing `pyproject.toml` when provided as a source

---

_@charliermarsh_

## Summary

In the process of making a different change, I noticed that we parse this during source discovery, throw it away, then parse it again later.


---

_Label `internal` added by @charliermarsh on 2025-09-15 00:38_

---

_Marked ready for review by @charliermarsh on 2025-09-15 00:43_

---

_@konstin approved on 2025-09-15 07:47_

---

_Merged by @charliermarsh on 2025-09-15 14:07_

---

_Closed by @charliermarsh on 2025-09-15 14:07_

---

_Branch deleted on 2025-09-15 14:07_

---
