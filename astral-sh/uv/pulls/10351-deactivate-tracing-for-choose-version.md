```yaml
number: 10351
title: Deactivate tracing for choose version
type: pull_request
state: merged
author: konstin
labels:
  - performance
assignees: []
merged: true
base: main
head: konsti/dont-trace-hottest-functions
created_at: 2025-01-07T09:44:03Z
updated_at: 2025-01-07T09:55:07Z
url: https://github.com/astral-sh/uv/pull/10351
synced_at: 2026-01-10T11:44:44Z
```

# Deactivate tracing for choose version

---

_Pull request opened by @konstin on 2025-01-07 09:44_

Ref https://github.com/astral-sh/uv/issues/10344

This trace is crucial to understanding the tracing-durations-export diagrams, but it took ~5% resolver thread runtime for apache-airflow (profiling information). It looks neutral in hyperfine (below noise threshold)

---

_Label `performance` added by @konstin on 2025-01-07 09:44_

---

_Merged by @konstin on 2025-01-07 09:55_

---

_Closed by @konstin on 2025-01-07 09:55_

---

_Branch deleted on 2025-01-07 09:55_

---
