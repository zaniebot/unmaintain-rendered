```yaml
number: 5675
title: "Formatter: try-except-else comment switches body"
type: issue
state: closed
author: konstin
labels:
  - bug
  - formatter
assignees: []
created_at: 2023-07-11T07:44:23Z
updated_at: 2023-08-16T07:09:48Z
url: https://github.com/astral-sh/ruff/issues/5675
synced_at: 2026-01-12T15:54:45Z
```

# Formatter: try-except-else comment switches body

---

_@konstin_

We format
```python
try:
    pass
except X:
    pass
else:
    # comment
    print()
```
as
```python
try:
    pass
except X:
    pass
    # comment
else:
    print()
```
even though the comment should rather remain a leading comment of the print statement

---

_Label `formatter` added by @konstin on 2023-07-11 07:44_

---

_Label `bug` added by @MichaReiser on 2023-07-11 07:52_

---

_Comment by @charliermarsh on 2023-08-01 01:11_

@konstin - I can't reproduce this, maybe it was fixed between now and then?

---

_Comment by @charliermarsh on 2023-08-01 01:11_

(Preemptively closing.)

---

_Closed by @charliermarsh on 2023-08-01 01:11_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-08-16 07:09_

---
