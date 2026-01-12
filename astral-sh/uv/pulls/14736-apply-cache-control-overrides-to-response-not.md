```yaml
number: 14736
title: Apply Cache-Control overrides to response, not request headers
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/cache-control
created_at: 2025-07-18T17:43:08Z
updated_at: 2025-07-18T20:32:31Z
url: https://github.com/astral-sh/uv/pull/14736
synced_at: 2026-01-12T16:11:22Z
```

# Apply Cache-Control overrides to response, not request headers

---

_@charliermarsh_

## Summary

This was just an oversight on my part in the initial implementation.

Closes https://github.com/astral-sh/uv/issues/14719.

## Test Plan

With:

```toml
[project]
name = "foo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13.2"
dependencies = [
]

[[tool.uv.index]]
url = "https://download.pytorch.org/whl/cpu"
cache-control = { api = "max-age=600" }
```

Ran `cargo run lock -vvv` and verified that the PyTorch index response was cached (whereas it typically returns `cache-control: no-cache,no-store,must-revalidate`).


---

_Label `bug` added by @charliermarsh on 2025-07-18 17:43_

---

_Marked ready for review by @charliermarsh on 2025-07-18 17:43_

---

_@zanieb approved on 2025-07-18 17:51_

---

_Merged by @charliermarsh on 2025-07-18 20:32_

---

_Closed by @charliermarsh on 2025-07-18 20:32_

---

_Branch deleted on 2025-07-18 20:32_

---
