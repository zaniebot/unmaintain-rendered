```yaml
number: 6131
title: "Add fast-path for `select_candidate` with singletons"
type: issue
state: closed
author: charliermarsh
labels:
  - performance
assignees: []
created_at: 2024-08-16T01:00:49Z
updated_at: 2024-09-09T13:43:59Z
url: https://github.com/astral-sh/uv/issues/6131
synced_at: 2026-01-10T04:45:09Z
```

# Add fast-path for `select_candidate` with singletons

---

_Issue opened by @charliermarsh on 2024-08-16 01:00_

We already know the version, and the version maps are indexed by version. Can we just skip the iterator?

In `echo "apache-airflow[google]" | cargo run pip compile -`, about 4-5% of the calls to `select_candidate` use a singleton.

---

_Label `performance` added by @charliermarsh on 2024-08-16 01:00_

---

_Label `internal` added by @charliermarsh on 2024-08-16 01:00_

---

_Renamed from "Add fast-path for `select_candidate` with singletones" to "Add fast-path for `select_candidate` with singletons" by @charliermarsh on 2024-08-16 01:00_

---

_Label `internal` removed by @charliermarsh on 2024-08-16 01:06_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-09 00:42_

---

_Closed by @charliermarsh on 2024-09-09 13:43_

---
