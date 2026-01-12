```yaml
number: 3132
title: Avoid treating localhost URLs as local file paths
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/localhost
created_at: 2024-04-19T00:30:32Z
updated_at: 2024-04-19T00:37:34Z
url: https://github.com/astral-sh/uv/pull/3132
synced_at: 2026-01-12T16:05:26Z
```

# Avoid treating localhost URLs as local file paths

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/3128.

## Test Plan

- `python -m http.server`
- `cargo run pip install "http://localhost:8000/werkzeug-3.0.2-py3-none-any.whl"`
- `cargo run pip install "http://localhost:8000/werkzeug-3.0.2-py3-none-any.whl"`


---

_Label `bug` added by @charliermarsh on 2024-04-19 00:30_

---

_Marked ready for review by @charliermarsh on 2024-04-19 00:30_

---

_Merged by @charliermarsh on 2024-04-19 00:37_

---

_Closed by @charliermarsh on 2024-04-19 00:37_

---

_Branch deleted on 2024-04-19 00:37_

---
