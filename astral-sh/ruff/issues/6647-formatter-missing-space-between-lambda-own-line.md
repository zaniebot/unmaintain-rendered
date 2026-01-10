```yaml
number: 6647
title: "Formatter: Missing space between lambda own line comment"
type: issue
state: closed
author: MichaReiser
labels:
  - bug
  - formatter
assignees: []
created_at: 2023-08-17T10:43:12Z
updated_at: 2023-08-18T15:34:56Z
url: https://github.com/astral-sh/ruff/issues/6647
synced_at: 2026-01-10T11:09:48Z
```

# Formatter: Missing space between lambda own line comment

---

_Issue opened by @MichaReiser on 2023-08-17 10:43_

Input 

```python
(
    lambda  
    # 3
    : None 
)
```

Output:

```python
(
    lambda: None# 3
)
```

**Expected**:

Input:
```python
(
    lambda  # 1
    # 2
    : # 3
    # 4
    None # 5
)
```

Output
```
(
    lambda:  # 1
    # 2
    # 3
    # 4
    None # 5
)
```

---

_Label `bug` added by @MichaReiser on 2023-08-17 10:43_

---

_Label `formatter` added by @MichaReiser on 2023-08-17 10:43_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-08-17 10:43_

---

_Comment by @MichaReiser on 2023-08-17 10:43_

Related to https://github.com/astral-sh/ruff/issues/6646


---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-18 03:33_

---

_Comment by @charliermarsh on 2023-08-18 03:35_

Interestingly, I can't get Black to parse either of these (though they are valid syntax).

---

_Closed by @charliermarsh on 2023-08-18 15:34_

---
