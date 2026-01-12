```yaml
number: 10276
title: Ignore empty or missing hrefs in Simple HTML
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - compatibility
assignees: []
merged: true
base: main
head: charlie/ignore
created_at: 2025-01-02T16:40:13Z
updated_at: 2025-01-02T17:43:16Z
url: https://github.com/astral-sh/uv/pull/10276
synced_at: 2026-01-12T16:09:12Z
```

# Ignore empty or missing hrefs in Simple HTML

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/7735.

## Test Plan

`cargo run pip install -f https://whl.smartgic.io/ ggwave --python-platform linux` (fails prior to this PR; passes after)


---

_Label `bug` added by @charliermarsh on 2025-01-02 16:40_

---

_Label `compatibility` added by @charliermarsh on 2025-01-02 16:40_

---

_Comment by @goldyfruit on 2025-01-02 17:31_

Thanks for this @charliermarsh :+1: 

---

_Merged by @charliermarsh on 2025-01-02 17:43_

---

_Closed by @charliermarsh on 2025-01-02 17:43_

---

_Branch deleted on 2025-01-02 17:43_

---
