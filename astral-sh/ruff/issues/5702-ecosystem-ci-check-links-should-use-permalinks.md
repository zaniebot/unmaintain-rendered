```yaml
number: 5702
title: Ecosystem CI check links should use permalinks
type: issue
state: closed
author: charliermarsh
labels:
  - internal
assignees: []
created_at: 2023-07-12T04:18:23Z
updated_at: 2023-07-12T06:26:39Z
url: https://github.com/astral-sh/ruff/issues/5702
synced_at: 2026-01-12T15:54:45Z
```

# Ecosystem CI check links should use permalinks

---

_@charliermarsh_

I notice that one of the Airflow links pointed to an irrelevant line of code. I'm guessing that the code changed between the ecosystem CI run and my clicking through. Could we instead use permalinks, so that they point to the relevant violations in perpetuity? (Is that even possible?)

---

_Label `internal` added by @charliermarsh on 2023-07-12 04:18_

---

_Comment by @dhruvmanila on 2023-07-12 04:46_

Yeah, it should be possible. We just need to get the current commit hash at the time of checkout.

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-07-12 04:46_

---

_Unassigned @dhruvmanila by @dhruvmanila on 2023-07-12 05:34_

---

_Closed by @zanieb on 2023-07-12 06:26_

---
