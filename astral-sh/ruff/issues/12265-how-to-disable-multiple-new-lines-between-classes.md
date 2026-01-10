```yaml
number: 12265
title: How to disable multiple new lines between classes and functions?
type: issue
state: closed
author: Filyus
labels:
  - question
assignees: []
created_at: 2024-07-10T04:19:47Z
updated_at: 2024-07-10T04:28:04Z
url: https://github.com/astral-sh/ruff/issues/12265
synced_at: 2026-01-10T11:09:54Z
```

# How to disable multiple new lines between classes and functions?

---

_Issue opened by @Filyus on 2024-07-10 04:19_

I want this because of fewer overall lines:
````python
def fn_1:
  pass

def fn_2:
  pass
  
def fn_3:
  pass 
````

but Ruff gives me this:
````python
def fn_1:
  pass


def fn_2:
  pass
  
  
def fn_3:
  pass 
````
Is there way to fix it?

---

_Comment by @charliermarsh on 2024-07-10 04:27_

Unfortunately we don't support that. It would be inconsistent with both [PEP 8](https://peps.python.org/pep-0008/#blank-lines) and other formatters like Black.

---

_Label `question` added by @charliermarsh on 2024-07-10 04:28_

---

_Closed by @charliermarsh on 2024-07-10 04:28_

---
