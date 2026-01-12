```yaml
number: 3993
title: Enhance ERA001 commented out code analysis
type: issue
state: closed
author: mikeleppane
labels:
  - question
assignees: []
created_at: 2023-04-17T06:45:51Z
updated_at: 2023-05-08T22:20:39Z
url: https://github.com/astral-sh/ruff/issues/3993
synced_at: 2026-01-12T15:54:44Z
```

# Enhance ERA001 commented out code analysis

---

_@mikeleppane_

Would it be possible to detect commented out code in case you have used triple-quoted strings? It probably won't be trivial thing to do because how to separate from an actual doc string. 

Ruff detects this: 
```python
# print("commented out code")
``` 

but not this:  
```python
""" print("commented out code") """
``` 

Ruff version:
```shell 
ruff 0.0.261
``` 

Thanks for the Ruff! We recently switched from flake8 to ruff for our production code. 



---

_Comment by @charliermarsh on 2023-05-08 22:20_

Thank you for the kind words :) I think I understand where the request is coming from, but I'd rather keep the rule as-is for now -- it seems less common to temporarily disable code by stringifying it, which is the mistake that this rule is intended to catch, and we'd be more likely to flag false positives here as you suggested.


---

_Closed by @charliermarsh on 2023-05-08 22:20_

---

_Label `question` added by @charliermarsh on 2023-05-08 22:20_

---
