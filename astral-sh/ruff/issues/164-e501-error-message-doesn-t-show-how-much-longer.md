```yaml
number: 164
title: "E501: error message doesn't show how much longer the line is"
type: issue
state: closed
author: Jackenmen
labels: []
assignees: []
created_at: 2022-09-12T02:07:30Z
updated_at: 2022-09-12T02:54:12Z
url: https://github.com/astral-sh/ruff/issues/164
synced_at: 2026-01-12T15:54:40Z
```

# E501: error message doesn't show how much longer the line is

---

_@Jackenmen_

Flake8 shows both the max line length and the actual length of the line:
```
❯ flake8 repro.py --max-line-length 88
repro.py:1:89: E501 line too long (110 > 88 characters)
```
Ruff does not:
```
❯ ruff repro.py  
repro.py:1:89: E501 Line too long

Found 1 error(s).
```

---

_Closed by @charliermarsh on 2022-09-12 02:54_

---
