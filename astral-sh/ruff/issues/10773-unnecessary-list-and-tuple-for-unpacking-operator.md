```yaml
number: 10773
title: Unnecessary list and tuple for unpacking operator
type: issue
state: open
author: spaceby
labels:
  - rule
assignees: []
created_at: 2024-04-04T08:51:47Z
updated_at: 2024-04-05T19:34:13Z
url: https://github.com/astral-sh/ruff/issues/10773
synced_at: 2026-01-12T15:54:50Z
```

# Unnecessary list and tuple for unpacking operator

---

_@spaceby_

RUF005 leaves a similar code after autofix

``` python
[*list(range(10)), 10]
```
It can be rewritten as
``` python
[*range(10), 10]
```

---

_Label `rule` added by @charliermarsh on 2024-04-05 19:34_

---
