```yaml
number: 318
title: Avoid unnecessarily fetching non-marker-required first-party dependencies
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/visit
created_at: 2023-11-04T17:00:55Z
updated_at: 2023-11-04T17:03:44Z
url: https://github.com/astral-sh/uv/pull/318
synced_at: 2026-01-10T15:50:28Z
```

# Avoid unnecessarily fetching non-marker-required first-party dependencies

---

_Pull request opened by @charliermarsh on 2023-11-04 17:00_

E.g., given:

```
flask; python_version < '3.7'
requests
```

We shouldn't request the metadata for Flask when on Python versions 3.7 or later.

---

_Merged by @charliermarsh on 2023-11-04 17:03_

---

_Closed by @charliermarsh on 2023-11-04 17:03_

---

_Branch deleted on 2023-11-04 17:03_

---
