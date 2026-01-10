```yaml
number: 6646
title: "Formatter: Drops trailing same line comment after `lambda :`"
type: issue
state: closed
author: MichaReiser
labels:
  - bug
  - formatter
  - help wanted
assignees: []
created_at: 2023-08-17T10:40:01Z
updated_at: 2023-08-18T15:34:56Z
url: https://github.com/astral-sh/ruff/issues/6646
synced_at: 2026-01-10T11:09:48Z
```

# Formatter: Drops trailing same line comment after `lambda :`

---

_Issue opened by @MichaReiser on 2023-08-17 10:40_

Input
```python
(
    lambda 
    : # 3
     None 
)
```

Output
```python
(lambda: None)
```

See how the comment `# 3` is missing

---

_Label `bug` added by @MichaReiser on 2023-08-17 10:40_

---

_Label `formatter` added by @MichaReiser on 2023-08-17 10:40_

---

_Label `help wanted` added by @MichaReiser on 2023-08-17 10:40_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-08-17 10:40_

---

_Renamed from "Formatter: Ruff drops trailing lambda `:` comment" to "Formatter: Ruff drops trailing same line comment after `lambda :`" by @MichaReiser on 2023-08-17 10:40_

---

_Renamed from "Formatter: Ruff drops trailing same line comment after `lambda :`" to "Formatter: Drops trailing same line comment after `lambda :`" by @MichaReiser on 2023-08-17 10:43_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-18 03:33_

---

_Closed by @charliermarsh on 2023-08-18 15:34_

---
