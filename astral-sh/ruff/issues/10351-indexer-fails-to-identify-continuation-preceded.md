```yaml
number: 10351
title: "`Indexer` fails to identify continuation preceded by newline"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2024-03-12T01:18:04Z
updated_at: 2024-03-12T04:35:42Z
url: https://github.com/astral-sh/ruff/issues/10351
synced_at: 2026-01-12T15:54:50Z
```

# `Indexer` fails to identify continuation preceded by newline

---

_@charliermarsh_

Given:

```python
(
    1
    \
    + 2)
```

We don't identify any `continuation_lines` in the `Indexer`.


---

_Label `bug` added by @charliermarsh on 2024-03-12 01:18_

---

_Label `accepted` added by @charliermarsh on 2024-03-12 01:18_

---

_Label `accepted` removed by @charliermarsh on 2024-03-12 01:18_

---

_Closed by @charliermarsh on 2024-03-12 04:35_

---
