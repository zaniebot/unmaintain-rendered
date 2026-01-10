```yaml
number: 2337
title: "Update `install_extra_index_url_has_priority` to avoid packaging breakage"
type: pull_request
state: merged
author: charliermarsh
labels:
  - testing
assignees: []
merged: true
base: main
head: charlie/ex
created_at: 2024-03-10T13:32:48Z
updated_at: 2024-03-10T13:50:20Z
url: https://github.com/astral-sh/uv/pull/2337
synced_at: 2026-01-10T14:54:43Z
```

# Update `install_extra_index_url_has_priority` to avoid packaging breakage

---

_Pull request opened by @charliermarsh on 2024-03-10 13:32_

`packaging==24.0` came out which broke this test. It has to run without `--exclude-newer` since it's testing an index that doesn't support it. Instead, though, we can just disable dependencies, since the test still exercises the same logic.

---

_Label `testing` added by @charliermarsh on 2024-03-10 13:32_

---

_Merged by @charliermarsh on 2024-03-10 13:50_

---

_Closed by @charliermarsh on 2024-03-10 13:50_

---

_Branch deleted on 2024-03-10 13:50_

---
