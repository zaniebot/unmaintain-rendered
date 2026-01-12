```yaml
number: 14831
title: Preserve index URL priority order when writing to pyproject.toml
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/add
created_at: 2025-07-22T18:45:14Z
updated_at: 2025-07-22T19:10:00Z
url: https://github.com/astral-sh/uv/pull/14831
synced_at: 2026-01-12T16:11:26Z
```

# Preserve index URL priority order when writing to pyproject.toml

---

_@charliermarsh_

## Summary

A little nuanced, but... When you add multiple `--index` URLs on the CLI (e.g., in `uv pip install`), we check the first-provided index, then the second index, etc. However, when we _write_ those URLs to the `pyproject.toml` in `uv add`, we were adding them in reverse-order. We now add them in a way that preserves the priority order.

Closes https://github.com/astral-sh/uv/issues/14817.


---

_Label `bug` added by @charliermarsh on 2025-07-22 18:45_

---

_Marked ready for review by @charliermarsh on 2025-07-22 18:45_

---

_@zanieb approved on 2025-07-22 18:49_

---

_Merged by @charliermarsh on 2025-07-22 19:09_

---

_Closed by @charliermarsh on 2025-07-22 19:09_

---

_Branch deleted on 2025-07-22 19:10_

---
